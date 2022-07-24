# Kubelet

The kubelet is installed in each Worker nodes and is the sole point of contact to the apiserver.

## Responsibilities

The kubelet:
- Registers the node with the kubernetes cluster
- Requests the container runtime (docker, containerd...) when it receives instructions to load a container or a pod on a node in order to pull the required image and run an instance of it.
- Monitors the state of the pods and containers in it and reports to the kube-apiserver on a timely basis.

## Installation

/!\ The kubelet is not installed by kubeadm /!\

For each node it needs to be:
- manually downloaded, extracted and installed
- run as a service

For detailed installation see the cluster setup section.