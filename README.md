# install ingress-nginx via helm
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx
```
# install cert-manager
```
kubectl create ns cert-manager
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.yaml
kubectl apply -f issuer.yaml
```
# external DNS
```
kubectl create ns external-dns

gcloud projects add-iam-policy-binding driven-manifest-306006 \
    --role='roles/dns.admin' \
    --member='serviceAccount:external-dns@driven-manifest-306006.iam.gserviceaccount.com'
	
gcloud iam service-accounts keys create credentials.json \
    --iam-account external-dns@driven-manifest-306006.iam.gserviceaccount.com
	
kubectl -n external-dns create secret \
    generic external-dns \
  --from-file=credentials.json=credentials.json

helm install external-dns \
  --set provider=google \
  --set google.project=driven-manifest-306006 \
  --set domainFilters[0]=ratner.ml \
  --set google.serviceAccountSecretKey=external-dns \
  --set rbac.create=true \
  --set logLevel=debug \
  -n external-dns \
  bitnami/external-dns


kubectl logs -f `kubectl get pods -n external-dns -o jsonpath="{.items[0].metadata.name}"` -n external-dns
```

# ambassador example
```
kubectl create namespace ambassador
helm repo add datawire https://www.getambassador.io
helm repo update
helm install ambassador --namespace ambassador datawire/ambassador
kubectl apply -f ambassador-example.yml
```