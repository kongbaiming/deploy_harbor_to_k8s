# 使用 harbor-helm将harbor部署至k8s

#### 先决条件
- [x]  helm
- [x]  storageClass
- [x]  已配置的kubectl
- [x]  镜像库
#### 目录结构说明

```
deploy_harbor_to_k8s
├── README.md
├── doc          #说明文档
├── harbor-helm  #harbor helm仓库
├── harbor_image #harbor 所需image
├── img    
├── nfs_external #nfs 动态存储部署文件目录
└── tools helm客户端以及服务端镜像,nfs-client镜像
```
#### 推送harbor镜像至镜像库

切换至 harbor_image 目录下    
将 load.sh 中 10.10.64.88 修改为你的仓库地址.

```
sh load.sh
```

#### 推送helm server 至镜像库
切换至 harbor-helm 目录下    
将 load.sh 中 10.10.64.88 修改为你的仓库地址.

```
sh load.sh
```

#### 动态卷存储(NFS)
参照doc/使用nfs作为k8s的动态存储 进行配置

#### 部署配置helm
参照 doc/helm配置部署 进行配置

#### 部署harbor至k8s集群
参照 doc/k8s上部署harbor 进行配置