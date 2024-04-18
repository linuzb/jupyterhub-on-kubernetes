# 前置准备

- [[kubernetes]] 部署
- [Helm | Installing Helm](https://helm.sh/docs/intro/install/) 安装 helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## NFS provider

[[搭建NFS provider]]

## Longhorn provider

[[longhorn 存储]]

## 创建secret

给 client-id 和 client-secret 创建secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: githubsecret
  namespace: jhub
type: Opaque
data:
  clientId: xxx
  clientSecret: xxx
  oauthCallbackUrl: xxx
```

# 安装


```bash
alias helm='helm --kubeconfig /home/linuzb/Projects/kube-cert/kubeconfig'
```

[values.yaml](https://github.com/jupyterhub/zero-to-jupyterhub-k8s/blob/main/jupyterhub/values.yaml)

```shell
# Let helm the command line tool know about a Helm chart repository
# that we decide to name jupyterhub.
helm repo add jupyterhub https://hub.jupyter.org/helm-chart/
helm repo update


# 查看版本
helm search repo jupyterhub -l



# 从仓库安装
helm upgrade --cleanup-on-fail \
  --install jupyterhub jupyterhub/jupyterhub \
  --namespace jhub \
  --create-namespace \
  --version=3.3.6 \
  --values config.yaml

# 从本地安装
# 下载到本地
helm pull jupyterhub/jupyterhub --version 3.3.6

tar -zxvf jupyterhub-3.3.6.tgz jupyterhub

helm upgrade --cleanup-on-fail \
  --install jupyterhub ./jupyterhub \
  --namespace jhub \
  --create-namespace \
  --version=3.3.6 \
  --values config.yaml
```