# mindcircuit13 - SAMPLE APP

 this are the steps for the CD PROCESS
creat argocd 
# kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# kubectl get pods -n argocd
# kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
