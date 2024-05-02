# WhatsApp Bridge

Deploy Whatsapp bridge:
```bash
helm upgrade -i mautrix-whatsapp halkeye/mautrix-whatsapp \
  --namespace matrix-synapse \
  --values mautrix-whatsapp-values.yml
```

Generate registration file:
```bash
mautrix-whatsapp -g -c config.yaml -r registration-new.yaml
```

Change domain name:
```bash
sed -i 's/example.com/domain.in/g' new-mautrix-whatsapp-values.yml
```
> generate file with correct domain or it won't work.

Copy registration file to matrix-synapse:
```bash
export POD_NAME=$(kubectl get pods --namespace matrix-synapse -l "app.kubernetes.io/name=matrix-synapse,app.kubernetes.io/instance=matrix-synapse,app.kubernetes.io/component=synapse" -o jsonpath="{.items[0].metadata.name}")

kubectl exec --namespace matrix-synapse $POD_NAME -- mkdir /synapse/data/bridges

kubectl cp whatsapp-registration.yaml $POD_NAME:/synapse/data/bridges/whatsapp-registration.yaml
```
