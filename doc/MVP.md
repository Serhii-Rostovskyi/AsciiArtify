# MVP (Minimum Viable Product)

## Introduce
The Minimum Viable Product (MVP) of the AsciiArtify project is geared towards launching a fundamental application via ArgoCD. This application will monitor changes from the GitHub repository go-demo-app and automatically synchronize them onto a Kubernetes cluster.

## Demo video
[![Watch the video](https://i.imgur.com/FwUCnbF.png)](https://youtu.be/DAebPqYb-Os)
Or click https://youtu.be/DAebPqYb-Os

## Steps to Set Up MVP

### 1. Establishing a Kubernetes Cluster with k3d
- Utilize k3d to create a local Kubernetes cluster.
- Confirm the cluster's operational status.
```bash
  k3d cluster create demo
  kubectl cluster-info
```
### 2. Installing ArgoCD
- Follow the official ArgoCD documentation to install ArgoCD on the Kubernetes cluster.
- Ensure successful deployment of ArgoCD pods.
```bash
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Configuring Application in ArgoCD
Download the Argo CD CLI.
Integrate the go-demo-app repository into ArgoCD for continuous delivery.
Set up automatic synchronization of repository changes to the Kubernetes cluster.
For detailed installation instructions, refer to the [CLI installation documentation.](https://argo-cd.readthedocs.io/en/stable/cli_installation/ "CLI installation documentation.").
```bash
  kubectl config set-context --current --namespace=argocd
  argocd login --core
  argocd app create demo --repo https://github.com/den-vasyliev/go-demo-app.git --path helm  --dest-server https://kubernetes.default.svc --dest-namespace demo
```

### 4. Demonstrate Application Deployment
4. Demonstrating Application Deployment
 - Deploy the go-demo-app using ArgoCD onto the Kubernetes cluster.
 - Monitor the synchronization process and ensure successful application deployment.
```bash
  argocd app get demo
  argocd app sync demo
```
## Demo Guidelines
- Use port-forwarding to access the ArgoCD UI:
```bash
  argocd admin initial-password -n argocd
  kubectl port-forward svc/argocd-server -n argocd 8088:443
```
- Open a web browser and navigate to [https://localhost:8088](https://localhost:8088) to access the ArgoCD UI.
- Navigate to the ArgoCD UI and observe the deployment status of the go-demo-app.
- Verify that changes from the GitHub repository are automatically synchronized and deployed onto the Kubernetes cluster.
- Enable autosync demo for AgroCD and control changes in Git repo.
- Test Application:
```bash
  kubectl port-forward svc/ambassador -n demo2 8090:80
  curl localhost:8090
  curl -F 'image=@/Users/andii.kozachenko/Downloads/Google_Images_2015_logo.svg.png' localhost:8090/img/
```
## Conclusion
The MVP demonstrates the automated deployment of the go-demo-app using ArgoCD on a Kubernetes cluster provisioned with k3d. This setup enables the AsciiArtify team to efficiently manage application deployments and iterate on features based on user feedback.
