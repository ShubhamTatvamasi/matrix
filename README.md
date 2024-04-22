# matrix

Add helm repos:
```bash
helm repo add halkeye https://halkeye.github.io/helm-charts/
helm repo add jetstack https://charts.jetstack.io
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
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


Install synapse:
```bash
helm upgrade -i synapse halkeye/synapse \
  --set homeserver.server_name=matrix.example.com \
  --set postgresql.enabled=true \
  --set homeserver.macaroon_secret_key=$(openssl rand -hex 32)
```
