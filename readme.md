## Initial ingress setup 
#### update ingress for easily approach 
run: kubectl get svc -n kube-system ( for list service run)

## add domain for dashboard
#### 1. Make sure with ingress 
run: kubectl get pods -n ingress-nginx

#### 2. make cluster issuer and cert-manager 
run: kubectl apply -f cluster-issuer.yml
result: clusterissuer.cert-manager.io/letsencrypt-prod created

#### 3. create kubernetes-dashboard-ingress.yml 
```

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
 
```
run: kubectl apply -f argocd-ingress.yml ( for apply )

check certificate  
run: kubectl get certificate -n argocd

run: kubectl get ingress -n argocd


#### Generate token for access kubernetes dashboard
-start generate token 
run: kubectl -n kubernetes-dashboard create token admin-user --duration=24h