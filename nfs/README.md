环境
- 节点ip: 172.16.0.100/24
- 节点地址：/data/nfs

```shell
echo -e "/data/nfs\t172.16.0.100/24(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports
```

```shell
/sbin/showmount -e 172.16.0.100
```


```shell
helm upgrade --install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=172.16.0.100 \
  --set nfs.path=/data/nfs \
  --set storageClass.onDelete=true \
  --set image.repository=registry.cn-hangzhou.aliyuncs.com/linuzb/nfs-subdir-external-provisioner

```

参考：

- [kubernetes-sigs/nfs-subdir-external-provisioner: Dynamic sub-dir volume provisioner on a remote NFS server. (github.com)](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner)
- [How to Setup Dynamic NFS Provisioning in a Kubernetes Cluster | by Hakan Bayraktar | Medium](https://hbayraktar.medium.com/how-to-setup-dynamic-nfs-provisioning-in-a-kubernetes-cluster-cbf433b7de29)