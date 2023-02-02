# Kubertnetes Dashboard and Metrics Server

<br />

## Enable Kubernetes single-node cluster on Docker-Desktop 
* Kubernetes can be enabled from Kubernetes settings panel
* Check the Enable Kubernetes box and press Apply & Restart 
* (Optional) Check the Use Rosetta for x86/amd64 emulation on Apple Silicon box
* And Enable the virtualization framework in General panel then press Apply and Restart

<br />

## Get the Docker-Desktop node's ip address
```yaml
cat /etc/hosts
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
```
<br />

## Project Overview
<p>
Dashboard is a web-based Kubernetes user interface. You can use Dashboard to deploy containerized applications to a Kubernetes cluster, troubleshoot your containerized application, and manage the cluster resources. You can use Dashboard to get an overview of applications running on your cluster, as well as for creating or modifying individual Kubernetes resources (such as Deployments, Jobs, DaemonSets, etc). For example, you can scale a Deployment, initiate a rolling update, restart a pod or deploy new applications using a deploy wizard.

Dashboard also provides information on the state of Kubernetes resources in your cluster and on any errors that may have occurred.
</p>

<br />

## Install Kubernetes Dashboard 
* Deploy Kubernetes Dashboard yaml
```yaml
  wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
  kubectl apply -f ./dashboard/recommended.yaml
```  
* Create a Service Account
```yaml
  kubectl apply -f ./dashboard/adminuser.yaml
```
* Create a ClusterRoleBinding
```yaml
  kubectl apply -f ./dashboard/dashboard-adminuser.yaml
```
* Get a Bearer Token
```yaml
  kubectl -n kubernetes-dashboard create token admin-user
```
* Run kubectl proxy
```yaml
  kubectl proxy
```
* Open the browser and navigate to URL
```yaml
  http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
* Select Token and paste the token created earlier and press Sign In
```yaml
  eyJhbGciOiJSUzI1NiIsImtpZCI6InpuOUFUQlU5c1FxU1Ewa0VNWHNBMWJhVl........
```
* You are now logged in with an admin

<br />

## Install Kubernetes Metric Server
* Install Helm
```yaml
  brew update
  brew install helm
```
* Add metrics-server repository
```yaml
  helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
  helm repo update
```
* Create namespace metrics
```yaml
  kubectl create namespace metrics
```
* Install metrics-server helm chart
```yaml
  helm install metrics-server metrics-server/metrics-server --version 3.8.3 --namespace metrics --set args={"--kubelet-insecure-tls=true"}
```
* Check the rollout status
```yaml
  kubectl -namespace metrics rollout status deployment metrics-server
```
* Check the pods
```yaml
  kubectl get pods --namespace metrics
```
<br />

### Get top pods and nodes
```yaml
kubectl top pods
kubectl top node
```
<br />

### Clean up everything
```yaml
kubectl -n kubernetes-dashboard delete serviceaccount admin-user
kubectl -n kubernetes-dashboard delete clusterrolebinding admin-user
kubectl delete -n kubernetes-dashboard
kubectl -n metrics delete deployment metrics-server
kubectl delete -n metrics 
```

<br />

## Stop and quit your Kubernetes cluster
* To pause or quit click on the Moby icon in Upper right corner and chose either pause or stop 

<br />

## Links
* Deploying Kubernetes Dashboard: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
* Accessing Kubernetes Dashboard: https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md
* Kubernetes Metrics-Server: https://github.com/kubernetes-sigs/metrics-server
* Kubernetes Metrics-Server Releases: https://github.com/kubernetes-sigs/metrics-server/releases
* k8s official documentation: https://kubernetes.io/docs/home/


