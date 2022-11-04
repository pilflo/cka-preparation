# ETCD-BACKUP

## Quick Wins

### Backup

1. Type "Backup ETCD" in documentation search.
2. Find ["Operating etcd clusters for Kubernetes | Kubernetes"](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/) result.
3. Go down to ~25% of the total page or search for 'cacert/snapshot save' to find pre-made etcd command.

- Do not forget to check that *--endpoints, --cert, --cacert, --key* match the values found when doing ```kubectl describe po -n kube-system etcd-```

### Restore on Stacked ETCD (kubeadm install)

1. Follow the same commands as backup, replace ```save``` with ```restore``` and add a *--data-dir* flag with a new directory for restoring the snapshot.
2. Update the etcd.yaml static pod manifest at */etc/kubernetes/manifests/etcd.yaml*
3. Change the **volume** path to the new directory (not the volumeMount).
4. Wait for the etcd pod to rollout.

### Restore on External ETCD

The first steps are the same than restoring for Stacked ETCD.

Then,

- ```systemctl status etcd``` to get the path of the service config file.
- change the *--data-dir* path
- ```systemctl daemon-reload && systemctl restart etcd```
- Make sure etcd has the correct permissions on new dir ```chown -R etcd:etcd /var/lib/new-etcd-data```

### Endpoints

ETCD endpoints are the urls at which the control-plane can contact the ETCD cluster.

The etcd endpoints can be found with the *--listen-client-urls* property.  
It defaults to <https://127.0.0.1:2379>.
