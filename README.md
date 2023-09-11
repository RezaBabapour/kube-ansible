# Kubernetes Cluster Deployment and Management with Ansible

This playbooks and roles do the process of deploying, upgrading Kubernetes cluster. Also it can deloy kubernetes manifests on cluster.

## Introduction
This repository includes three main playbooks:

1. **deploy-cluster.yml**: This playbook is responsible for deploying a Kubernetes cluster on virtual machines (VMs). With this playbook, you can create a fresh Kubernetes cluster.

2. **upgrade-cluster.yml**: Use this playbook to upgrade your existing Kubernetes cluster to a specific version.

3. **deploy-stack.yml**: With this playbook you can apply Kubernetes manifest, such as deploying applications, services to cluster.

## Preparation

Before using the Ansible playbooks, you'll need to perform the following preparation steps:

1. **Create the Necessary VMs**:
   - Set up the required number of virtual machines (VMs) for your Kubernetes cluster. Ensure you have a mix of `controller` and `worker` nodes, with an odd number of controllers being preferable for fault tolerance.

2. **Clone the Project**:
   - Clone this repository to the `/opt/kube-ansible` directory on your `deployer` server:

   ```bash
   git clone git@github.com:RezaBabapour/kube-ansible.git /opt/kube-ansible

3. **Choose a Deployer Server**:
   - Select one of the VMs to serve as the `deployer` server. You'll use this server to execute Ansible playbooks. 
   - Copy the public key of the `deployer` server to all other servers to establish secure communication.
   - Install `ansible` on the `deployer` server.
   - Populate the inventory file as shown in `inventory/hosts.yml`.

4. **Populate the /etc/hosts File**:
   - On each VM, update the `/etc/hosts` file to define hostnames and IP addresses for proper network resolution. An example `/etc/hosts` file might look like this:

   ```plaintext
   192.168.20.2 kube01
   192.168.20.3 kube02
   192.168.20.4 kube03
   192.168.20.5 kube04

## Deploy Cluster

In this step, we deploy our Kubernetes cluster without using any automation tools like `kubeadm` & `kubespray`. For more details, refer to the deply cluster [technical documentation](deploy-cluster.md).

1. Specify `kubernetes.version` in `group_vars/all.yml`
2. Create `files` dir in project root for keeping needed binary files during the cluster setup. Download needed binaries and store them in recpective directory. Here is the `files` directory structure:
```bash
├── v1.25.13
│   ├── cni
│   ├── containerd
│   ├── crictl
│   ├── etcd
│   ├── kube-apiserver
│   ├── kube-controller-manager
│   ├── kubectl
│   ├── kubelet
│   ├── kube-proxy
│   ├── kube-scheduler
│   ├── runc
│   ├── salam
│   └── wget-log
└── v1.26.8
    ├── cni
    ├── containerd
    ├── crictl
    ├── etcd
    ├── kube-apiserver
    ├── kube-controller-manager
    ├── kubectl
    ├── kubelet
    ├── kube-proxy
    ├── kube-scheduler
    └── runc
```

3. Now you can execute the `deploy-cluster.yml` playbook:
```bash
ansible-playbook -i inventory/hosts.yml deploy-cluster.yml
```

## Upgrade Cluster

In this step, we upgrade our existing Kubernetes cluster. For more details, refer to the upgrade [technical documentation](upgrade-cluster.md).

1. Download your desired version binaries from https://www.downloadkubernetes.com/. Save them in files directory.
2. Edit `kubernetes.version` in `group_vars/all.yml`.
3. Execute `upgrade-cluster.yml`:
```bash
ansible-playbook -i inventory/hosts.yml upgrade-cluster.yml
```

## Deploy Stack

With `deploy-stack.yml` you could deploy any manifest on kunernetes cluster. For more details, refer to the deploy stack [technical documentation](deploy-stack.md).

1. First you must create manifests in jinja2 format and put them in `roles/pre-service/templates/stack_name`.
2. Execute `deploy-stack.yml` with stack name as a variable:
```bash
ansible-playbook -i inventory/hosts.yml -e service=efk deploy-stack.yml
