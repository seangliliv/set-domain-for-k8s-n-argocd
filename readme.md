## Initial ingress setup 
#### update ingress for easily approach 
run: kubectl get svc -n kube-system ( for list service run)
https://34.101.123.149:31001
## add domain for dashboard
#### 1. Make sure with ingress 
run: kubectl get pods -n ingress-nginx

#### 2. make cluster issuer and cert-manager 
run: kubectl apply -f cluster-issuer.yml
result: clusterissuer.cert-manager.io/letsencrypt-prod created

#### 3. create kubernetes-dashboard-ingress.yml 
```
apiVersion: networking.k8s.io/v1
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dashboard-ingress
  namespace: kubernetes-dashboard
  annotations:
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - kubernetes-dashboard.domain.com
    secretName: dashboard-tls
  rules:
  - host: kubernetes-dashboard.domain.site
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: kubernetes-dashboard
            port:
              number: 443
```
for apply it 
run: kubectl apply -f dashboard-ingress.yml

check certificate 
run: kubectl get certificate -n kubernetes-dashboard

start check ingress controller 
run: kubectl get ingress -n kubernetes-dashboard

#### let's start with argocd 
run: kubectl get pods -n argocd (for check it run or not)
#### 1. start 
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-ingress
  namespace: argocd
  annotations:
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-passthrough: "true"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - argocd.domain.com
    secretName: argocd-tls
  rules:
  - host: argocd.domain.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: argocd-server
            port:
              number: 443
```
run: kubectl apply -f argocd-ingress.yml ( for apply )

check certificate  
run: kubectl get certificate -n argocd

run: kubectl get ingress -n argocd


#### Generate token for access kubernetes dashboard
-start generate token 
run: kubectl -n kubernetes-dashboard create token admin-user --duration=24h