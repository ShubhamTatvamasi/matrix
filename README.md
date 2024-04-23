# matrix

Add helm repos:
```bash
helm repo add ananace-charts https://ananace.gitlab.io/charts
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
  --set serverName=matrix.synergyquantum.in \
  --set wellknown.enabled=true
```

Uninstall matrix-synapse:
```bash
helm un matrix-synapse --namespace matrix-synapse
kubectl delete ns matrix-synapse
```


