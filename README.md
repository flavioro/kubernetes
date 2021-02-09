![](https://miro.medium.com/max/700/0*0xAFVp2oiGROzPiX)
Commands and examples to kubernetes

Use extension Vs Code:
 - docker
 - kubernetes
 
## Tools
 - kubectl (tool cli in https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Concept
O Kubernetes é o orquestrador de clusters de contêineres mais 
popular atualmente no mercado. O cluster Kubernetes é formado
basicamente de 2 elementos:
<br> - A máquina Control Plane que gerencia o cluster;
<br> - E as máquinas Nodes - ou Nós - que executam as aplicações.

## Structure Kubernetes
<br> - Kubeadm: Responsável por iniciar o Kubernetes.
<br> - Kubelet: Ele é executado em todos os nós no cluster e responsável
por garantir que os pods e os contêineres estão sendo executados
corretamente.
<br> - Kubectl: Responsável por interagir com o cluster.


## Install tools to kubernetes (kubelet kubeadm kubectl) in ubuntu

*Add repositories kubernetes
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```

*Add packages
```
echo “deb http://apt.kubernetes.io/ kubernetes-xenial main” > / etc/apt/sources.list.d/kubernetes.list
```

* Update packages
```
apt-get update
```
* Install tools kubernetes
```
apt-get install -y kubelet kubeadm kubectl
```

* Inicialize kubernetes to Scaleway (scaleway.com), ONLY SERVER MASTER
```
kubeadm init --apiserver-cert-extra-sans dominioOrIp
```

* Other examples Inicialize kubernetes
https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/

* Command create file configuration, ONLY SERVER MASTER
```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

* Command run in other nodes 'TOKEN' is token generated
```
kubeadm join <ENDEREÇO_IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<TOKEN>
```

* Show cluster, run in Master
```
kubectl get nodes
```

* Install interface network, ONLY MASTER
```
kubectl apply -f “https://cloud.weave.works/k8s/net?k8sversion=$(kubectl version | base64 | tr -d ‘\n’)”
```
* Show structure cluster
```
kubectl get all --all-namespaces
```

 *Create Deployment
 ```
 kubectl apply -f nginx-deployment.yaml
 ```
 Or directory current and recursive
 ```
 kubectl apply -f .\ -R 
 ```

 
 ## Run enviroment local (simulation)
 ![](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/07751442-deployment.png)
 * Tool kind (https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
 ```
 kind create cluster
 ```
 Or create with name
 ```
 kind create cluster --name mycluster
 ```

* After installation local, show in Docker
```
docker container ls
```

* Show info cluster, last parameter name cluster (kind-mycluster)
```
kubectl cluster-info --context kind-mycluster
```
Or

```
kubectl cluster-info
```

* Delete cluster
```
kind delete cluster
```
Or

```
kind delete cluster --name mycluster
```

<br>List all pods
```
kubectl get pod
```
<br>Delete pod
```
kubectl delete pod namePod
```
<br>List all ReplicaSet
```
kubectl get pod replicaset
```
* Details, to create file .yaml, indentation must be correct and the field name must be lowercase.
```
metadata:
  name: myreplicaset 
```
<br>Detail ReplicaSet Describe
```
kubectl describe replicaset <Name_replicaset>
```
<br>List all services
```
kubectl get services
```
<br>Detail pod
```
kubectl describe pod <Name_POD>
```
* Use Port Forwarding to Access Applications in a Cluster
```
# pod/name_pod and port_local to port_pod
kubectl port-forward pod/myreplicaset-dd5lf 8080:80
```

### Create multi nodes with kind, use file configuration, see more details https://kind.sigs.k8s.io/docs/user/configuration/
* Create file configuration 'config.yaml'
```
kind create cluster --name multinode --config .\config.yaml 
```


