# install ingress-nginx via helm
- helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
- helm repo update
- helm install ingress-nginx ingress-nginx/ingress-nginx

# install cert-manager
- kubectl create ns cert-manager
- kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.yaml