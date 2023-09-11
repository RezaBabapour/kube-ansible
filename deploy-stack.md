# Deploy Stack

The `deploy-stack.yml` playbook applies Kubernetes manifests to one of the controller nodes.

## Create Manifests

To deploy Kubernetes manifests using the `deploy-stack.yml` playbook, adhere to the following steps:

1. **Store Manifests**: Save your manifests in the `roles/pre-service/templates` directory. Each stack should be stored in a separate directory, such as the `efk` directory.

2. **Ansible Deployment**: Ansible will move the manifests to the controller server and subsequently apply them.

## EFK Manifests

EFK (Elasticsearch, Fluentd, Kibana) includes an Elasticsearch statefulset, Fluentd daemonset, and Kibana deployment. During the controller node setup, we created a local storage class. For the Elasticsearch statefulset, you only need to create a persistent volume and persistent volume claim (PVC).

The number of Elasticsearch instances and persistent volumes is automatically managed by Ansible. Each worker node hosts one instance of Elasticsearch and its corresponding local volume.
