# j-burg11-cluster-2 kubernetes cluster

## Resources
- https://github.com/kubernetes-sigs/kubespray/blob/master/docs/vars.md
- https://github.com/kubernetes-sigs/kubespray/blob/master/docs/ansible.md
- https://github.com/kubernetes-sigs/kubespray/blob/master/docs/upgrades.md


## Summary
This is the second cluster
- Should probably do this:
    - `ssh-copy-id user@192.168.1.1`
- Duplicated group_vars from kubespray v2.18.0
- Ran `sudo pip3 install -r requirements.txt` in kubespray repo
- Edited `hosts.yml`
- Enabled addons in `group_vars/k8s_cluster/addons.yml`
    - ingress_nginx_enabled
    - cert-manager
    - helm_enabled
    - local_volume_provisioner
- `group_vars/k8s_cluster/k8s-cluster.yml`
    - name
    - ingress_nginx_enabled
- `group_vars/all/all.yml`
    - upstream_dns_servers
- `group_vars/etcd.yml``
    - etcd_memory_limit

Ran
`ansible-playbook -i ../infrastructure/Kubernetes/j-burg11-k8cluster-2/hosts.yml cluster.yml --ask-become-pass -b -v`


Upgraded with
`ansible-playbook upgrade-cluster.yml -b -i --ask-become-pass ../infrastructure/ansible/j-burg11-k8cluster-2/hosts.yml`


## Storage

NFS

```
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/

helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --set nfs.server=192.168.1.96 \
    --set nfs.path=/mnt/user/appdata/j-burg11-cluster-2-storage
```
SMB
```
helm repo add csi-driver-smb https://raw.githubusercontent.com/kubernetes-csi/csi-driver-smb/master/charts

helm install csi-driver-smb csi-driver-smb/csi-driver-smb --namespace kube-system --version v1.6.0

kubectl create secret generic smbcreds --from-literal username=USERNAME --from-literal password="PASSWORD"
```