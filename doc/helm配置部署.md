- 客户端配置

注：请确保该机器，可操作k8s集群

切换目录至tools下
```
# tar xf helm-v2.14.3-linux-amd64.tar.gz
# cp linux-amd64/helm /usr/local/bin
# chmod +x /usr/local/bin/helm
```
- 服务端配置 

```
# 初始化
# helm init  --upgrade  --service-account tiller --skip-refresh  -i 10.10.64.88/tools/registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.14.3kubectl create serviceaccount --namespace kube-system tiller

# kubectl create serviceaccount --namespace kube-system tiller

# kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

# kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'

```
