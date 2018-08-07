# minikube v0.28.1

由于不能从 google container 上直接 pull 镜像，所以这里通过 docker hub 的 Automated Builds 功能从项目的 dockerfile 中 Build 到 docker 的官方服务器上，然后再从它们上面拉取.

##	kube 1.10.0 需要的镜像:
```
REPOSITORY                                               		TAG
k8s.gcr.io/kube-apiserver-amd64             					v1.10.1
k8s.gcr.io/kube-controller-manager-amd64   						v1.10.1
k8s.gcr.io/kube-proxy-amd64                						v1.10.1
k8s.gcr.io/kube-scheduler-amd64            						v1.10.1
k8s.gcr.io/kubernetes-dashboard-amd64                   		v1.8.1
k8s.gcr.io/etcd-amd64                      						3.1.12
k8s.gcr.io/pause-amd64                     						3.1
k8s.gcr.io/kube-addon-manager                           		v8.6
k8s.gcr.io/k8s-dns-sidecar-amd64                                1.14.5
k8s.gcr.io/k8s-dns-kube-dns-amd64                               1.14.5
k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64                          1.14.5
k8s.gcr.io/defaultbackend                               		1.4
quay.io/kubernetes-ingress-controller/nginx-ingress-controller  0.16.2

```

## docker hub上设置
由于docker hub不能后期更改一个image的tag，所以每次更新kubernetes时，都在build settings中，手动增加一个版本对应文件

## 更改tag
```
images=(kube-proxy-amd64:v1.5.3 kube-scheduler-amd64:v1.5.3 kube-controller-manager-amd64:v1.5.3 kube-apiserver-amd64:v1.5.3 etcd-amd64:3.0.14-kubeadm kube-discovery-amd64:1.0 pause-amd64:3.0 kubedns-amd64:1.9 dnsmasq-metrics-amd64:1.0 kube-dnsmasq-amd64:1.4 exechealthz-amd64:1.2)
for imageName in ${images[@]} ; do
  docker pull ist0ne/$imageName
  docker tag ist0ne/$imageName gcr.io/google_containers/$imageName
  docker rmi ist0ne/$imageName
done

images=(heapster:canary heapster_grafana:v3.1.1 heapster_influxdb:v0.7)
for imageName in ${images[@]} ; do
  docker pull ist0ne/$imageName
  docker tag ist0ne/$imageName kubernetes/$imageName
done
```

