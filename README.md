![](https://geekflare.com/wp-content/uploads/2020/05/Img1DocAndKub.png)
![](https://i.octopus.com/docs/deployments/kubernetes/deploy-container/deploy-container.svg)

### Resume
 - Extension docker and kubernetes to VsCode;
 - Tool CLI;
 - Concept;
 - Structure Kubernetes;
 
 ```* Linux Ubuntu```
 - Tools to Ubuntu (kubelet, kubeadm, kubectl);
 - Settings and install to ubuntu;
 - Install to Master and Node
 
 ```* Local enviroment (Dev)```
 - Settings and Install to enviroment local (Dev)
 - kind (tool to enviroment local)
 - Manager enviroment local with files .yaml
 - kubectl and your functions
 - Create, Delete, Update to pod
 - Use replicaset, deployment and service
 - Replicas, port-forword
 - Create pod with .yaml example
 - Resilience, scalability  
 - Create replicaset with .yaml example
 - Create deployment with .yaml example
 - Examples files .yaml to service, deployment, replicaset and pod
 - Example deployment and service to mongodb
 - Example deployment and service to api (deployment.yaml and service.yaml)
 

## Use extension Vs Code:
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
<br> More about service https://cloud.google.com/kubernetes-engine/docs/concepts/service

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

 *Create Deployment kubectl apply -f nameFile.yaml
 ```
 kubectl apply -f nginx-deployment.yaml
 ```
 Or directory current and recursive
 ```
 kubectl apply -f .\ -R 
 ```
The kubectl command line tool lets you control Kubernetes clusters
 
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
<br>Delete pod Or Delete replicaset Or Delete deployment
```
kubectl delete pod namePod
kubectl delete replicaset nameReplicaset
```
<br>List all ReplicaSet
```
kubectl get replicaset
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

### History Deployment, control versions
```
kubectl rollout history deployment nameDeployment 
```

* Rollback Deployment, 
```
kubectl rollout undo deployment nameDeployment
```

### Example create pod with .yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: web
      image: kubedevio/nginx-color:blue
      ports:
        - containerPort: 80
```

### Example create replicaset with .yaml
``` yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myreplicaset
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx-color
  template:
    metadata:
      labels:
        app: nginx-color
    spec:
      containers:
        - name: web
          image: kubedevio/nginx-color:green
          ports:
            - containerPort: 80

```

### Example create deployment with .yaml
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-color
  template:
    metadata:
      labels:
        app: nginx-color
    spec:
      containers:
        - name: web
          image: kubedevio/nginx-color:green
          ports:
            - containerPort: 80

```

### Example create service with .yaml, used app: nginx-color
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: service-nginx
spec:
  selector:
    app: nginx-color
  ports:
    - port: 80
      protocol: TCP
  type: LoadBalancer


```

### Manager pods, need to use deployment with file .yaml
```
kubectl aplly -f .\deployment.yaml
```

### Example deployment and service to mongodb
deployment.yaml
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  # name cannot contain capital letters 
  name: deploy-mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
        # name of image hub.docker and version
          image: mongo:4.2.8
          ports:
            - containerPort: 27017
          env:
            # variables environment to username and password
            - name: MONGO_INITDB_ROOT_USERNAME
              value: mongouser
            - name: MONGO_INITDB_ROOT_PASSWORD
              value: mongopwd

```

service.yaml
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
    # port service
    - port: 27017
      # port pod mongodb
      targetPort: 27017
  type: ClusterIP
```

