# Readme
ArgoCD Kubernetes automation of https://github.com/ognyan-dossev/address-book
## Kubernetes setup on WSL2
### Apply dashboard manifest and edit login permissions
Add `- --enable-skip-login`
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
kubectl delete clusterrolebinding serviceaccount-cluster-admin
kubectl create clusterrolebinding serviceaccount-cluster-admin --clusterrole=cluster-admin --user=system:serviceaccount:kubernetes-dashboard:kubernetes-dashboard
kubectl edit deployment kubernetes-dashboard -n kubernetes-dashboard
```
Start proxy in new terminal
```
kubectl proxy
```
Dashboard URL: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
### 

## ArgoCD
Install ArgoCD in k8s
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```


Access ArgoCD UI
```
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8002:443 -n argocd
```

Login with admin user and below token (as in documentation):
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo
```
You can change and delete init password.

