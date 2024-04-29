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
