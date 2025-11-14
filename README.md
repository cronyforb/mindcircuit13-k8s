CD PROCESSS

 Step 1: Install ArgoCD
bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Step 2: Wait for pods to be ready
bash
kubectl get pods -n argocd -w
Wait until all pods show Running status (1-2 minutes)

Step 3: Change Service to LoadBalancer
bash
kubectl patch svc argocd-server -n argocd -p '{"spec":{"type":"LoadBalancer"}}'
Step 4: Get ArgoCD URL and Password
bash
# Get the LoadBalancer URL (wait 1-2 minutes for it to be assigned)
kubectl get svc argocd-server -n argocd

# Get the admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
Step 5: Create prod namespace
bash
kubectl create namespace prod
Step 6: Access ArgoCD Web UI
Copy the EXTERNAL-IP or hostname from kubectl get svc argocd-server -n argocd

Open it in your browser: http://<loadbalancer-url>

Login with:

Username: admin

Password: <password-from-step-4>

Step 7: Create Application in ArgoCD UI
Click "+ Create Application"

Fill in:

Application Name: my-app

Project: default

Sync Policy: Automated

Repository URL: <your-git-repo-url>

Path: <path-to-manifests>

Cluster URL: https://kubernetes.default.svc

Namespace: prod

Click "Create"

Step 8: Check your application
bash
# Check all resources in prod namespace
kubectl get all -n prod

# Get the application service URL (if you have a service)
kubectl get svc -n prod
Complete Command Sequence:
bash
# 1. Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 2. Wait for pods
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=argocd-server -n argocd --timeout=300s

# 3. Expose via LoadBalancer
kubectl patch svc argocd-server -n argocd -p '{"spec":{"type":"LoadBalancer"}}'

# 4. Get credentials
echo "ArgoCD URL:"
kubectl get svc argocd-server -n argocd -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
echo "Password:"
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# 5. Create prod namespace
kubectl create namespace prod
