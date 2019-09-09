修改 harbor-helm中values.yaml文件
修改 storageClass 的值为 你所创建的storageClass name
创建storageClass 请参考doc/使用nfs作为k8s的动态存储.pdf

```
# cat values.yaml |  grep repository:
    repository: 10.10.64.88/goharbor/nginx-photon
    repository: 10.10.64.88/goharbor/harbor-portal
    repository: 10.10.64.88/goharbor/harbor-core
    repository: 10.10.64.88/goharbor/harbor-jobservice
      repository: 10.10.64.88/goharbor/registry-photon
      repository: 10.10.64.88/goharbor/harbor-registryctl
    repository: 10.10.64.88/goharbor/chartmuseum-photon
    repository: 10.10.64.88/goharbor/clair-photon
      repository: 10.10.64.88/goharbor/notary-server-photon
      repository: 10.10.64.88/goharbor/notary-signer-photon
      repository: 10.10.64.88/goharbor/harbor-db
      repository: 10.10.64.88/goharbor/redis-photon

# cat values.yaml |  grep storageClas 
      storageClass: "managed-nfs-storage"
      storageClass: "managed-nfs-storage"
      storageClass: "managed-nfs-storage"
      storageClass: "managed-nfs-storage"
      storageClass: "managed-nfs-storage"
```

切换至 harbor-helm 目录下
```
# helm install --name harbor  --set expose.type=nodePort,expose.tls.enabled=true,expose.tls.commonName=harbor --namespace kube-system .
kubectl get pod,svc -n kube-system

NAME                                               READY   STATUS      RESTARTS   AGE
pod/calico-kube-controllers-8597459c5d-pblqt       1/1     Running     0          3d2h
pod/calico-node-5ff74                              1/1     Running     0          3d2h
pod/calico-node-sm2fs                              1/1     Running     0          3d2h
pod/calico-node-wcsbx                              1/1     Running     0          3d2h
pod/coredns-86c84f8f7b-vpkbx                       1/1     Running     0          3d2h
pod/coredns-autoscaler-fcbd767c9-j875v             1/1     Running     0          3d2h
pod/harbor-harbor-chartmuseum-7c7d49fd94-lwhdl     1/1     Running     0          82m
pod/harbor-harbor-clair-86d5fdb86b-l85c8           1/1     Running     1          82m
pod/harbor-harbor-core-674554789b-2dd9c            1/1     Running     1          82m
pod/harbor-harbor-database-0                       1/1     Running     0          82m
pod/harbor-harbor-jobservice-6c5d9dff4c-s78xz      1/1     Running     2          82m
pod/harbor-harbor-nginx-867f65478f-fcmjb           1/1     Running     0          82m
pod/harbor-harbor-notary-server-865d4496f4-vj2hz   1/1     Running     2          82m
pod/harbor-harbor-notary-signer-84dfc4cffd-f6qv9   1/1     Running     2          82m
pod/harbor-harbor-portal-744547f86b-g26xh          1/1     Running     0          82m
pod/harbor-harbor-redis-0                          1/1     Running     0          82m
pod/harbor-harbor-registry-5d8f668b6f-nqvlk        2/2     Running     0          82m
pod/metrics-server-77bc67577d-cvrzm                1/1     Running     0          3d2h
pod/rke-coredns-addon-deploy-job-7265n             0/1     Completed   0          3d2h
pod/rke-ingress-controller-deploy-job-9x4jb        0/1     Completed   0          3d2h
pod/rke-metrics-addon-deploy-job-4qnvl             0/1     Completed   0          3d2h
pod/rke-network-plugin-deploy-job-9xv99            0/1     Completed   0          3d2h
pod/tiller-deploy-7b84cc46ff-2gz5p                 1/1     Running     0          89m

NAME                                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                     AGE
service/harbor                        NodePort    10.43.214.146   <none>        80:30002/TCP,443:30003/TCP,4443:30004/TCP   82m
service/harbor-harbor-chartmuseum     ClusterIP   10.43.142.48    <none>        80/TCP                                      82m
service/harbor-harbor-clair           ClusterIP   10.43.89.45     <none>        6060/TCP,6061/TCP                           82m
service/harbor-harbor-core            ClusterIP   10.43.10.222    <none>        80/TCP                                      82m
service/harbor-harbor-database        ClusterIP   10.43.133.5     <none>        5432/TCP                                    82m
service/harbor-harbor-jobservice      ClusterIP   10.43.40.194    <none>        80/TCP                                      82m
service/harbor-harbor-notary-server   ClusterIP   10.43.86.52     <none>        4443/TCP                                    82m
service/harbor-harbor-notary-signer   ClusterIP   10.43.186.228   <none>        7899/TCP                                    82m
service/harbor-harbor-portal          ClusterIP   10.43.2.2       <none>        80/TCP                                      82m
service/harbor-harbor-redis           ClusterIP   10.43.155.229   <none>        6379/TCP                                    82m
service/harbor-harbor-registry        ClusterIP   10.43.249.233   <none>        5000/TCP,8080/TCP                           82m
service/kube-dns                      ClusterIP   10.43.0.10      <none>        53/UDP,53/TCP,9153/TCP                      3d2h
service/metrics-server                ClusterIP   10.43.73.121    <none>        443/TCP                                     3d2h
service/tiller-deploy                 ClusterIP   10.43.33.15     <none>        44134/TCP                                   4h47m
```

状态为runing后,通过浏览器访问  
https://{k8s_node_ip}:30003/harbor/projects  
user: admin
passwd: Harbor12345