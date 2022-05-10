# ArgoCD

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.

## Installation
Before installing argocd, you need a kubernetes cluster. You can use minikube or kind cluster.

#### Installation of Minikube on Ubuntu 18.04 / 20.04 / 22.04
https://computingforgeeks.com/how-to-install-minikube-on-ubuntu-debian-linux/

#### Installation of Kind Cluster
https://kind.sigs.k8s.io/docs/user/quick-start/

#### Create a namespace named "argocd"
kubectl create namespace argocd

### Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#### Download and Install argocd CLI
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd

#### Grab the initial admin password and copy the displayed password somewhere. This is necessary to change the password later
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

#### Do port forwarding since argocd server is running inside kubernetes on ClusterIP. Run as background process
kubectl port-forward svc/argocd-server -n argocd --address=0.0.0.0 8443:443 > /dev/null 2>&1 &

#### Check if the port forward command is running in the background
jobs -l

### Login to argocd server using IP or hostname
argocd login localhost:8443

#### The above command will display a warning. Never mind, just press y
**WARNING:** server certificate had error: x509: certificate signed by unknown authority. Proceed insecurely (y/n)?

username: admin
password: <Refer to line 23 above>

#### Change admin password. You have to enter the old password and enter the new password twice. Minimum length of new password must be 8
argocd account update-password

Password: <Refer to line 23 above>
Enter new password twice and confirm

#### Launch argocd server site.
http://<ip_address_or_hostname>:8443

#### Login using the credentials
username: admin
Password: <updated_password - Refer to line 41 above>

## Sample Deployment
In this repository, you will find kubernetes manifests and helm charts for mongodb and mongo-express deployments.

However, these can be changed to point to different applications based on the requirments.

Also you will find a file named mongodb-argpcd-app.yaml. This is an ArgoCD yaml for application deployments. This 
can be changed to deploy different applications. You can also configure the sync policy to either manual or automatic.







