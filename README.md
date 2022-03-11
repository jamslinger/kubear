# Kubear 
Kubear is an Ansible playbook that creates a ready-to-use Kubernetes 
bare metal cluster that's meant to be running on Raspberry Pis. It aims to provide the 
cluster with as little preparatory work as possible. Once rolled out,
you'll have a fully functional cluster with the following components:

- Container runtime: [containerd](https://github.com/containerd/containerd)
- CNI: [Cilium](https://cilium.io/) 
- [Sealed-Secrets](https://github.com/bitnami-labs/sealed-secrets)

## Preliminaries
The nodes must be initially accessible via the ansible ssh configuration
in the inventory group vars. If you add a new user via the users role you  
can change the ansible config after the first successful run. Also make 
sure the correct ssh port is still configured as this might have changed
by the security role while configuring ssh. To avoid to accidentally lock 
yourself out, it's probably best to have a shell open on your nodes while
performing changes to the ssh config.

## Getting Started
Download the imported roles:
```
ansible-galaxy install -r requirements.yml
```

Specify all the hosts that should form the cluster in your inventory
hosts file. There are three groups available:
- `[k8s_master]`: Should only contain a single entry for the master node (runs `kubeadm init`)
- `[k8s_cp]`: Nodes that should join the cluster as additional control-plane nodes
- `[k8s_worker]`: Nodes that should join the cluster as worker nodes

If you want to initialize a new cluster, make sure
to set `join_only: false`. Set `join_only: true` if a cluster is 
already running, and you want to join a new node.

If you want to reset and recreate the cluster from scratch set `reset_cluster: true`.

## Tested Hardware
Raspberry Pi 4 Model B - 8GB

**OS:** Ubuntu Server 20.04

**Arch:** ARM64