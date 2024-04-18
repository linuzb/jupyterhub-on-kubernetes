
# longhorn 存储

[longhorn/longhorn: Cloud-Native distributed storage built on and for Kubernetes (github.com)](https://github.com/longhorn/longhorn?tab=readme-ov-file)

# 依赖

[Installation Requirements](https://longhorn.io/docs/1.6.1/deploy/install/#installation-requirements)

## ubuntu

检查脚本

```shell
curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.6.1/scripts/environment_check.sh | bash
```

安装依赖

```shell
apt-get install open-iscsi

apt-get install nfs-common
```

[kubectl 安装](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)及 kubeconfig 配置

# 部署

## helm

[Longhorn | Documentation](https://longhorn.io/docs/1.6.1/deploy/install/install-with-helm/)

```bash
helm pull longhorn/longhorn --version 1.6.1

helm upgrade --install longhorn ./longhorn \
  --namespace longhorn-system \
  --create-namespace \
  --version 1.6.1 \
  --values config.yaml
```

config.yaml

```yaml
defaultSettings:
  # 配置默认数据存放地址
  defaultDataPath: /data/longhorn

service:
  ui:
    # -- Service type for Longhorn UI. (Options: "ClusterIP", "NodePort", "LoadBalancer", "Rancher-Proxy")
    type: NodePort
    # -- NodePort port number for Longhorn UI. When unspecified, Longhorn selects a free port between 30000 and 32767.
    nodePort: 30180
  manager:
    # -- Service type for Longhorn Manager.
    type: NodePort
    # -- NodePort port number for Longhorn Manager. When unspecified, Longhorn selects a free port between 30000 and 32767.
    nodePort: 30181

longhornManager:
  # -- Toleration for Longhorn Manager on nodes allowed to run Longhorn Manager.
  tolerations:
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"

longhornDriver:
  # -- Toleration for Longhorn Driver on nodes allowed to run Longhorn components.
  tolerations: 
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"
```

# 页面

[Longhorn dashboard](http://172.16.0.100:30180/#/dashboard)

# 参考

- [17. Kubernetes - 持久化存储（Longhorn） - Dy1an - 博客园 (cnblogs.com)](https://www.cnblogs.com/Dy1an/p/17245825.html)
