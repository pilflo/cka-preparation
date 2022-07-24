# Kube-apiserver

The Kube-apiserver is at the center of all the different tasks done in the cluster.

It is a REST API and is the primary interface with the end user.

## Responsibilities

The Kube-apiserver:
- Authenticates user when is receives a request
- Validates the user's request
- Retrieve the data from ETCD
- Updates ETCD in case of change
- Is continuously monitored by the kube-scheduler to apply changes
- After updating ETCD, passes the changes to the kubelet(s) so the change is reflected on worker nodes

The kube-apiserver is the only component that directly interacts with ETCD.

## Installation 

- If you use kubeadm you don't need to worry about kube-apiserver installation.
- If you install your cluster the hard way, you need to download the binary and configure its execution through a systemctl service.