# matrix

Add helm repos:
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add ananace-charts https://ananace.gitlab.io/charts
helm repo add jetstack https://charts.jetstack.io
```

Install ingress-nginx:
```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --create-namespace \
  --namespace ingress-nginx \
  --set controller.ingressClassResource.default=true
```

Install cert-manager:
```bash
helm install cert-manager jetstack/cert-manager \
  --create-namespace \
  --namespace cert-manager \
  --set installCRDs=true
```

Install ClusterIssuer:
```yaml
kubectl apply -f - << EOF
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-issuer
    solvers:
    - http01:
        ingress:
          ingressClassName: nginx
EOF
```

Install frpc:
```bash
kubectl -n ingress-nginx apply -f https://raw.githubusercontent.com/ShubhamTatvamasi/matrix/master/frpc-configmap.yml
kubectl -n ingress-nginx apply -f https://raw.githubusercontent.com/ShubhamTatvamasi/frp/master/k8s/client/deployment.yml
```

Install matrix-synapse:
```bash
helm upgrade -i matrix-synapse ananace-charts/matrix-synapse \
  --create-namespace \
  --namespace matrix-synapse \
  --set serverName=matrix.example.com \
  --set wellknown.enabled=true \
  --set ingress.annotations."cert-manager\.io/cluster-issuer=letsencrypt-prod" \
  --set ingress.tls[0].secretName=matrix-example-com-tls \
  --set ingress.tls[0].hosts[0]=matrix.example.com
```

You can create a user in your new Synapse install by running the following command: `(replacing USERNAME and PASSWORD)`
```bash
export POD_NAME=$(kubectl get pods --namespace matrix-synapse -l "app.kubernetes.io/name=matrix-synapse,app.kubernetes.io/instance=matrix-synapse,app.kubernetes.io/component=synapse" -o jsonpath="{.items[0].metadata.name}")

kubectl exec --namespace matrix-synapse $POD_NAME -- register_new_matrix_user -c /synapse/config/homeserver.yaml -c /synapse/config/conf.d/secrets.yaml -u USERNAME -p PASSWORD --admin http://localhost:8008
```
> You can also specify --no-admin to create a non-admin user.

Uninstall matrix-synapse:
```bash
helm un matrix-synapse --namespace matrix-synapse
kubectl delete ns matrix-synapse
```


