#  Setup K8-Cluster using kubeadm [K8 Version-->1.28.1]
### 1. Run the script for package given in k8s-setup folder.
### 2. Next pulling config images (not a necessary step but helps work run quicker)
```bash
sudo kubeadm config images pull
```
### 3. Initialise the control plane which will generate token for worker nodes to join in the cluster.
```bash 
sudo kubeadm init \
  --apiserver-advertise-address=<PRIVATE-IP> \
  --pod-network-cidr=172.17.0.0/16
```
### 4. Configure a pod network in the cluster, for that run script calico.yaml given in k8s-setup folder.
  1- Install the Tigera Operator and custom resource definitions.
``` bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml

```
  2- Download the custom resources necessary to configure Calico.
```bash  
  curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources.yaml
```
  3- Create the manifest to install Calico.
```bash  
  kubectl create -f custom-resources.yaml
```
  4- Monitor the deployment
```bash  
 watch kubectl get tigerastatus
```
### 5: Verify cluster

```bash
kubectl get nodes
kubectl get pods -A
```








