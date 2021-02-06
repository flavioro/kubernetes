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
