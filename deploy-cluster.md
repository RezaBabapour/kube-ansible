# Deploy Cluster
The kubernetes cluster created from scratch. There is some steps to achive this goal, lets break `deplpy-cluster.yml` to smaller parts.

## Create needed certificates
For securing connection between K8s services we most create some certificates. This steps run by `certificate-ca` role.

1. **Create PKI**: To establish our custom certificates, we begin by creating Certificate Authority (CA) certificates. These CA certificates are generated only once. This means that if you rerun this step, Ansible **will not generate new CA certificates**.

2. **Create service certs**: Using CA we create certs for other parts like Controller Manager, Workers, Kube Proxy, Scheduler, ...
3. **Copy certs to servers**: We must copy certs to servers. Some to worker nodes and some to controller nodes.

## Copy binaries to nodes
This binaries is the most important part of k8s. Role `copy-bin` copy binaries to servers. There is 3 host group that this role copy their bins. Worker groups, Controller groups and Etcd bins.

## Create etcd cluster
At this point we can create etcd service on etcd host. role `etcd` has the service unit for etcd in its templates.

## Setup controller cluster
This step is similar to the previous one, with a few distinctions. In this phase, we need to select an API server load balancer. Instead of utilizing a load balancer like HAProxy, which introduces another potential single point of failure, the preferred option is to deploy kube-vip. However, for simplicity and time constraints, we are using the first server in the controller group as the API server. In this step we create some RBAC for connection between kubelet and api-server.

## Setup worker nodes
In this phase, we start by creating services for the binaries required on the worker nodes. Additionally, since pods run on these nodes, we create a bridged interface to facilitate communication among them.

## Setup CNI & DNS
Kubernetes does not provide service discovery and networking between pods. In the `addons` role, we configure `CoreDNS` as the name server for service discovery and introduce `Calico` for pod-to-pod communication.
