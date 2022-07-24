# ETCD

ETCD is a distributed, reliable key-value store. It is simple, secure and fast.

Its role in Kubernetes is to store information regarding the cluster (Nodes, Pods, Configs, Secrets, Account ...).
Every information you get from a kubectl command comes from ETCD.

## 2 ways to deploy

### Manual

With the manual setup, ETCD is deployed by the administrator separately from Kubernetes installation.
We then need to inform the kube api-server about the address to reach in order to communicate with ETCD.

ETCD needs to be started as a service (etcd.service).
The address on which etcd listens must be passed when starting the service as *--advertise-client-urls* flag.


### Kubeadm

With the Kubeadm setup, etcd is deployed automatically as a system pod (etcd-master in kube-system namespace).


## High Availability

When setting ETCD in HA environment, there are multiple master nodes, each one having an ETCD instance running on it.

The address of the instances must be passed when starting the service as *--initial-cluster* flag.

## ETCD startup command

You can check all the options passed when starting ETCD with the command:
```
kubectl describe pod <ETCD_POD_NAME> -n kube-system
>> OUTPUT <<
...
Command:
    etcd 
    --advertise-client-urls=https://172.18.0.3:2379
    --cert-file=/etc/kubernetes/pki/etcd/server.crt
    --client-cert-auth=true
    --data-dir=/var/lib/etcd
    --initial-advertise-peer-urls=https://172.18.0.3:2380
    --initial-cluster=local-control-plane=https://172.18.0.3:2380
    --key-file=/etc/kubernetes/pki/etcd/server.key
    --listen-client-urls=https://127.0.0.1:2379,https://172.18.0.3:2379
    --listen-metrics-urls=http://127.0.0.1:2381
    --listen-peer-urls=https://172.18.0.3:2380
    --name=local-control-plane
    --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    --peer-client-cert-auth=true
    --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    --snapshot-count=10000
    --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
...
```

It does provide information on listen urls, etcd instances address, certificates location ...

## etcdctl

*etcdctl* is the etcd client command line tool. It is useful for communicating with the etcd cluster through the ETCD API.

### Example commands

We know from the previous section where are located the etcd certificates.
```
--cacert /etc/kubernetes/pki/etcd/ca.crt     
--cert /etc/kubernetes/pki/etcd/server.crt     
--key /etc/kubernetes/pki/etcd/server.key
```

This way we can either
- Install etcdctl in our machine, get the certificates, get the address of our ETCD instance (and the multiple *endpoint* in case of HA multiple controle plane) and reach the ETCD API.
- Directly run etcdctl from the etcd pod in kube-system namespace.

#### List keys

```bash
kubectl exec <ETCD_POD_NAME> -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```

#### List cluster members

```bash
kubectl exec <ETCD_POD_NAME> -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl member list --write-out=table --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```

#### List endpoint status

```bash
kubectl exec <ETCD_POD_NAME> -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl endpoint status --write-out=table --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```