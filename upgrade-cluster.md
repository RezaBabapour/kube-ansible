# Upgrade Cluster

You can upgrade the cluster using the `upgrade-cluster.yml` playbook. The upgrade process consists of two steps: upgrading controller nodes followed by upgrading worker nodes.


## Upgrade Controllers

To maintain uninterrupted controller services, we perform upgrades on the controller nodes one at a time. Before starting the upgrade process, ensure you have downloaded the new binaries and saved them in the `files` directory.

1. **Stop Controller Services**: Begin by stopping controller services, which include kube-apiserver, kube-scheduler, and kube-controller-manager.

2. **Copy Binaries**: Copy the new binaries to the controller node and set their execute permissions.

3. **Start Controller Services**: After copying the binaries, start the controller services.

4. **Wait for Initialization**: Allow a minute for the services to initialize and stabilize before proceeding further.


## Upgrade Workers

Before do anything new binaries must be downloaded, Ansible upgrade one worker at time:

1. **Cordon Worker Node**: So no new pod scheduled on the node in the middle of upgrading.

2. **Drain Worker Nodes**: Use Kubernetes tools like `kubectl drain` to gracefully evict all pods from a worker node. This ensures minimal disruption during the upgrade.

3. **Stop Worker Services**: Stop the Kubernetes-related services on the worker node. These services may include `kubelet` and `kube-proxy`.

4. **Replace Binaries**: Copy the new binaries to the worker node and set the necessary execute permissions.

5. **Start Worker Services**: Restart the Kubernetes services on the worker node.

6. **Uncordon Worker Node**: Now pods can be scheduled to the worker node.
