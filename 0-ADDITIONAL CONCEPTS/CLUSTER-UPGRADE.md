# cluster-upgrade

## Disable Scheduling

```kubectl cordon NODENAME```

## Disable Scheduling AND Evict All

```kubectl drain NODENAME --ignore-daemonsets```

## Enable Scheduling

```kubectl uncordon NODENAME```

## Cluster Upgrade Steps

### Master Node

- Upgrade kubeadm

```bash
apt-get mark unhold kubeadm && apt-get update
apt-get upgrade kubeadm=1.25.0-00
```

- Upgrade cluster with kubeadm

```bash
kubeadm upgrade plan 
kubeadm upgrade apply v1.25.0
```

- Upgrade kubelet and kubectl

```bash
apt-mark unhold kubelet kubectl && apt-get update
apt-get upgrade -y kubelet=1.25.0-00 kubectl=1.25.0-00
```

- Restart kubelet

```bash
systemctl daemon-reload && systemctl restart kubelet
apt-mark hold kubeadm kubectl kubelet
kubectl uncordon controlplane
```
