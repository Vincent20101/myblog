# kubernets 集群安装部署。


## 安装 Docker
所有节点都需要安装 Docker。
```bash
apt-get update && apt-get install docker.io
```


## 安装 kubelet、kubeadm 和 kubectl

在所有节点上安装 kubelet、kubeadm 和 kubectl。

kubelet 运行在 Cluster 所有节点上，负责启动 Pod 和容器。

kubeadm 用于初始化 Cluster。

kubectl 是 Kubernetes 命令行工具。通过 kubectl 可以部署和管理应用，查看各种资源，创建、删除和更新各种组件。
```bash

apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get updateapt-get install -y kubelet kubeadm kubectl
```
 
 
这里是有阿里云的ubuntu源来安装k8s。
```bash
#!/bin/bash

apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 

cat << EOF > /etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF

apt-get update
apt-get install -y kubelet kubeadm kubectl

```
## 下载Docker镜像

这里需要几个docker的image，这里从镜像网站拉取images，直接执行 kubeadm init 拉取的images
是从k8s.gcr.io网站拉取的，这个网站国内访问不来的，除非你。。。。（这个敏感不能说出来。你懂得。）
```bash
#/bin/bash
# 从docker上拉取k8s的镜像
docker pull mirrorgooglecontainers/kube-apiserver:v1.12.2
docker pull mirrorgooglecontainers/kube-controller-manager:v1.12.2
docker pull mirrorgooglecontainers/kube-scheduler:v1.12.2
docker pull mirrorgooglecontainers/kube-proxy:v1.12.2
docker pull mirrorgooglecontainers/pause:3.1
docker pull mirrorgooglecontainers/etcd:3.2.24
docker pull kuberneter/coredns:1.2.2
# 拉取之后 要改个tag的名称的。
docker tag mirrorgooglecontainers/kube-apiserver:v1.12.2 k8s.gcr.io/kube-apiserver:v1.12.2
docker tag mirrorgooglecontainers/kube-controller-manager:v1.12.2 k8s.gcr.io/kube-controller-manager:v1.12.2
docker tag mirrorgooglecontainers/kube-scheduler:v1.12.2 k8s.gcr.io/kube-scheduler:v1.12.2
docker tag mirrorgooglecontainers/kube-proxy:v1.12.2 k8s.gcr.io/kube-proxy:v1.12.2
docker tag mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.1
docker tag mirrorgooglecontainers/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24
docker tag kuberneter/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2

# 这个后面碰到问题，需要拉取这个镜像
docker pull registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64
docker tag registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64


```

```bash

docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-proxy:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-apiserver:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-controller-manager:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-scheduler:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/etcd:3.2.24 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/coredns:1.2.2 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/flannel:v0.10.0-amd64 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/pause:3.1 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kubernetes-dashboard-amd64:v1.10.0


docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-proxy:v1.12.3 k8s.gcr.io/kube-proxy:v1.12.3 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-apiserver:v1.12.3 k8s.gcr.io/kube-apiserver:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-controller-manager:v1.12.3 k8s.gcr.io/kube-controller-manager:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-scheduler:v1.12.3 k8s.gcr.io/kube-scheduler:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64   ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/pause:3.1 k8s.gcr.io/pause:3.1  ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kubernetes-dashboard-amd64:v1.10.0 k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.0

```

```bash

#gcr.io/kubernetes-helm/tiller:v2.11.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.11.0
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.11.0 gcr.io/kubernetes-helm/tiller:v2.11.0
```

使用另外一种网络需要的docker镜像，开始是使用的flannel网络。

```bash
#quay.io/calico/node:v3.3.2
docker pull registry.cn-hangzhou.aliyuncs.com/liuq/calico-node:v2.6.2
docker tag  registry.cn-hangzhou.aliyuncs.com/liuq/calico-node:v2.6.2 quay.io/calico/node:v2.6.2

```

```bash

# k8s.gcr.io/heapster-amd64:v1.5.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4 k8s.gcr.io/heapster-amd64:v1.5.4

# k8s.gcr.io/heapster-grafana-amd64:v5.0.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4 k8s.gcr.io/heapster-grafana-amd64:v5.0.4

# k8s.gcr.io/heapster-influxdb-amd64:v1.5.2
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2 k8s.gcr.io/heapster-influxdb-amd64:v1.5.2
```

## 在k8s-master机器上执行kubeadm  init初始化。
```
root@k8s-master:~# kubeadm init --apiserver-advertise-address 10.0.63.47 --pod-network-cidr=10.244.0.0/16
I1121 21:55:58.472084    2033 version.go:93] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get https://dl.k8s.io/release/stable-1.txt: read tcp 10.0.63.47:48026->23.236.58.218:443: read: connection reset by peer
I1121 21:55:58.473142    2033 version.go:94] falling back to the local client version: v1.12.2
[init] using Kubernetes version: v1.12.2
[preflight] running pre-flight checks
[preflight/images] Pulling images required for setting up a Kubernetes cluster
[preflight/images] This might take a minute or two, depending on the speed of your internet connection
[preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[preflight] Activating the kubelet service
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Generated etcd/ca certificate and key.
[certificates] Generated etcd/server certificate and key.
[certificates] etcd/server serving cert is signed for DNS names [k8s-master localhost] and IPs [127.0.0.1 ::1]
[certificates] Generated etcd/healthcheck-client certificate and key.
[certificates] Generated apiserver-etcd-client certificate and key.
[certificates] Generated etcd/peer certificate and key.
[certificates] etcd/peer serving cert is signed for DNS names [k8s-master localhost] and IPs [10.0.63.47 127.0.0.1 ::1]
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [k8s-master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.0.63.47]
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
[certificates] Generated sa key and public key.
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests" 
[init] this might take a minute or longer if the control plane images have to be pulled
[apiclient] All control plane components are healthy after 32.503912 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.12" in namespace kube-system with the configuration for the kubelets in the cluster
[markmaster] Marking the node k8s-master as master by adding the label "node-role.kubernetes.io/master=''"
[markmaster] Marking the node k8s-master as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "k8s-master" as an annotation
[bootstraptoken] using token: s56myc.82qpolpdadevbt8r
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 10.0.63.47:6443 --token s56myc.82qpolpdadevbt8r --discovery-token-ca-cert-hash sha256:b075883d2963b624cfe8fe86ac9f7992724dc1af9fb912e1d637481722e6ccaf

root@k8s-master:~# 
```
执行完kubeadm  init之后会有个kubeadm  join命令的提示。这个要特别注意。


```
kubeadm join 10.0.63.47:6443 --token s56myc.82qpolpdadevbt8r --discovery-token-ca-cert-hash sha256:b075883d2963b624cfe8fe86ac9f7992724dc1af9fb912e1d637481722e6ccaf

# 这里的10.0.63.47ip是我们master节点的ip。
```

```
systemd,1
  ├─accounts-daemon,909
  │   ├─{gdbus},1181
  │   └─{gmain},1169
  ├─acpid,936
  ├─atd,931 -f
  ├─cron,939 -f
  ├─dbus-daemon,855 --system --address=systemd: --nofork --nopidfile --systemd-activation
  ├─dhclient,941 -1 -v -pf /run/dhclient.enp0s3.pid -lf /var/lib/dhcp/dhclient.enp0s3.leases -I -df /var/lib/dhcp/dhclient6.enp0s3.leases enp0s3
  ├─dockerd,1145 -H fd://
  │   ├─docker-containe,1280 -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --metrics-interval=0 --start-timeout 2m --state-dir /var/run/docker/libcontainerd/containerd --shim docker-containerd-shim --runtime docker-runc
  │   │   ├─docker-containe,2198 fd61fa0dfafd285fedc9f4555340077612acf45f427e1e52d774bb2dc9e90855 /var/run/docker/libcontainerd/fd61fa0dfafd285fedc9f4555340077612acf45f427e1e52d774bb2dc9e90855 docker-runc
  │   │   │   ├─pause,2270
  │   │   │   ├─{docker-containe},2199
  │   │   │   ├─{docker-containe},2200
  │   │   │   ├─{docker-containe},2201
  │   │   │   ├─{docker-containe},2232
  │   │   │   ├─{docker-containe},2233
  │   │   │   ├─{docker-containe},2239
  │   │   │   ├─{docker-containe},2240
  │   │   │   └─{docker-containe},2241
  │   │   ├─docker-containe,2205 ec0b0ac93389024f57cd2a6523c39e73d70b15f9cd68a72f2061c59056c6d22a /var/run/docker/libcontainerd/ec0b0ac93389024f57cd2a6523c39e73d70b15f9cd68a72f2061c59056c6d22a docker-runc
  │   │   │   ├─pause,2256
  │   │   │   ├─{docker-containe},2208
  │   │   │   ├─{docker-containe},2209
  │   │   │   ├─{docker-containe},2212
  │   │   │   ├─{docker-containe},2214
  │   │   │   ├─{docker-containe},2216
  │   │   │   ├─{docker-containe},2227
  │   │   │   ├─{docker-containe},2228
  │   │   │   └─{docker-containe},2231
  │   │   ├─docker-containe,2206 55d23bd9b85e9569a4b5b203017e077069bb0b928291d3573b7991bb0d08c02b /var/run/docker/libcontainerd/55d23bd9b85e9569a4b5b203017e077069bb0b928291d3573b7991bb0d08c02b docker-runc
  │   │   │   ├─pause,2285
  │   │   │   ├─{docker-containe},2210
  │   │   │   ├─{docker-containe},2211
  │   │   │   ├─{docker-containe},2213
  │   │   │   ├─{docker-containe},2215
  │   │   │   ├─{docker-containe},2217
  │   │   │   ├─{docker-containe},2226
  │   │   │   ├─{docker-containe},2229
  │   │   │   └─{docker-containe},2230
  │   │   ├─docker-containe,2244 b9f4c6938317993a45bbf58fa2e06127e8ac919e5866de37c920a4efcb45055d /var/run/docker/libcontainerd/b9f4c6938317993a45bbf58fa2e06127e8ac919e5866de37c920a4efcb45055d docker-runc
  │   │   │   ├─pause,2295
  │   │   │   ├─{docker-containe},2245
  │   │   │   ├─{docker-containe},2246
  │   │   │   ├─{docker-containe},2250
  │   │   │   ├─{docker-containe},2251
  │   │   │   ├─{docker-containe},2255
  │   │   │   ├─{docker-containe},2309
  │   │   │   ├─{docker-containe},2315
  │   │   │   └─{docker-containe},2316
  │   │   ├─docker-containe,2373 a3f9e6c912c28c2d4ca8f274a8913e0da077813827316407f82fa8237ee8de32 /var/run/docker/libcontainerd/a3f9e6c912c28c2d4ca8f274a8913e0da077813827316407f82fa8237ee8de32 docker-runc
  │   │   │   ├─kube-controller,2386 --address=127.0.0.1 --allocate-node-cidrs=true --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf --authorization-kubeconfig=/etc/kubernetes/controller-manager.conf --client-ca-file=/etc/kubernetes/pki/ca.crt --cluster-cidr=10.244.0.0/16 --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt --cluster-signing-key-file=/etc/kubernetes/pki/ca.key --controllers=*,bootstrapsigner,tokencleaner --kubeconfig=/etc/kubernetes/controller-manager.conf --leader-elect=true --node-cidr-mask-size=24 --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt --root-ca-file=/etc/kubernetes/pki/ca.crt --service-account-private-key-file=/etc/kubernetes/pki/sa.key --use-service-account-credentials=true
  │   │   │   │   ├─{kube-controller},2542
  │   │   │   │   ├─{kube-controller},2543
  │   │   │   │   ├─{kube-controller},2544
  │   │   │   │   ├─{kube-controller},2549
  │   │   │   │   └─{kube-controller},2621
  │   │   │   ├─{docker-containe},2374
  │   │   │   ├─{docker-containe},2375
  │   │   │   ├─{docker-containe},2376
  │   │   │   ├─{docker-containe},2377
  │   │   │   ├─{docker-containe},2378
  │   │   │   ├─{docker-containe},2399
  │   │   │   ├─{docker-containe},2401
  │   │   │   └─{docker-containe},2402
  │   │   ├─docker-containe,2404 2bb83de69e15e9211473b0d75fe0a93e536a0ae1ad9fd82dc1e313449c7b062c /var/run/docker/libcontainerd/2bb83de69e15e9211473b0d75fe0a93e536a0ae1ad9fd82dc1e313449c7b062c docker-runc
  │   │   │   ├─etcd,2418 --advertise-client-urls=https://127.0.0.1:2379 --cert-file=/etc/kubernetes/pki/etcd/server.crt --client-cert-auth=true --data-dir=/var/lib/etcd --initial-advertise-peer-urls=https://127.0.0.1:2380 --initial-cluster=k8s-master=https://127.0.0.1:2380 --key-file=/etc/kubernetes/pki/etcd/server.key --listen-client-urls=https://127.0.0.1:2379 --listen-peer-urls=https://127.0.0.1:2380 --name=k8s-master --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt --peer-client-cert-auth=true --peer-key-file=/etc/kubernetes/pki/etcd/peer.key --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt --snapshot-count=10000 --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
  │   │   │   │   ├─{etcd},2537
  │   │   │   │   ├─{etcd},2538
  │   │   │   │   ├─{etcd},2539
  │   │   │   │   ├─{etcd},2552
  │   │   │   │   ├─{etcd},2553
  │   │   │   │   ├─{etcd},2557
  │   │   │   │   └─{etcd},2558
  │   │   │   ├─{docker-containe},2406
  │   │   │   ├─{docker-containe},2407
  │   │   │   ├─{docker-containe},2408
  │   │   │   ├─{docker-containe},2409
  │   │   │   ├─{docker-containe},2410
  │   │   │   ├─{docker-containe},2431
  │   │   │   ├─{docker-containe},2433
  │   │   │   └─{docker-containe},2434
  │   │   ├─docker-containe,2454 8629702f1fd9894cb216ab93380f587ba700f59ff0468027d3453f2b60c62e76 /var/run/docker/libcontainerd/8629702f1fd9894cb216ab93380f587ba700f59ff0468027d3453f2b60c62e76 docker-runc
  │   │   │   ├─kube-scheduler,2479 --address=127.0.0.1 --kubeconfig=/etc/kubernetes/scheduler.conf --leader-elect=true
  │   │   │   │   ├─{kube-scheduler},2534
  │   │   │   │   ├─{kube-scheduler},2535
  │   │   │   │   ├─{kube-scheduler},2536
  │   │   │   │   ├─{kube-scheduler},2541
  │   │   │   │   ├─{kube-scheduler},2560
  │   │   │   │   └─{kube-scheduler},2564
  │   │   │   ├─{docker-containe},2456
  │   │   │   ├─{docker-containe},2457
  │   │   │   ├─{docker-containe},2460
  │   │   │   ├─{docker-containe},2462
  │   │   │   ├─{docker-containe},2464
  │   │   │   ├─{docker-containe},2494
  │   │   │   ├─{docker-containe},2499
  │   │   │   └─{docker-containe},2504
  │   │   ├─docker-containe,2455 7ac38e2ff6fdea4ec6be0fb34900b112fef9845a701e6467510522bc6341b0bc /var/run/docker/libcontainerd/7ac38e2ff6fdea4ec6be0fb34900b112fef9845a701e6467510522bc6341b0bc docker-runc
  │   │   │   ├─kube-apiserver,2495 --authorization-mode=Node,RBAC --advertise-address=10.0.63.47 --allow-privileged=true --client-ca-file=/etc/kubernetes/pki/ca.crt --enable-admission-plugins=NodeRestriction --enable-bootstrap-token-auth=true --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key --etcd-servers=https://127.0.0.1:2379 --insecure-port=0 --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key --requestheader-allowed-names=front-proxy-client --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt --requestheader-extra-headers-prefix=X-Remote-Extra- --requestheader-group-headers=X-Remote-Group --requestheader-username-headers=X-Remote-User --secure-port=6443 --service-account-key-file=/etc/kubernetes/pki/sa.pub --service-cluster-ip-range=10.96.0.0/12 --tls-cert-file=/etc/kubernetes/pki/apiserver.crt --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
  │   │   │   │   ├─{kube-apiserver},2545
  │   │   │   │   ├─{kube-apiserver},2546
  │   │   │   │   ├─{kube-apiserver},2547
  │   │   │   │   ├─{kube-apiserver},2565
  │   │   │   │   ├─{kube-apiserver},2567
  │   │   │   │   ├─{kube-apiserver},2568
  │   │   │   │   └─{kube-apiserver},4089
  │   │   │   ├─{docker-containe},2458
  │   │   │   ├─{docker-containe},2459
  │   │   │   ├─{docker-containe},2461
  │   │   │   ├─{docker-containe},2463
  │   │   │   ├─{docker-containe},2465
  │   │   │   ├─{docker-containe},2493
  │   │   │   ├─{docker-containe},2500
  │   │   │   └─{docker-containe},2503
  │   │   ├─docker-containe,2625 bca51977b449c740262639a3f8c4c5163e2a806ae208453d75f6da235af66046 /var/run/docker/libcontainerd/bca51977b449c740262639a3f8c4c5163e2a806ae208453d75f6da235af66046 docker-runc
  │   │   │   ├─pause,2638
  │   │   │   ├─{docker-containe},2626
  │   │   │   ├─{docker-containe},2627
  │   │   │   ├─{docker-containe},2628
  │   │   │   ├─{docker-containe},2629
  │   │   │   ├─{docker-containe},2630
  │   │   │   ├─{docker-containe},2651
  │   │   │   ├─{docker-containe},2653
  │   │   │   └─{docker-containe},2654
  │   │   ├─docker-containe,2667 146ec82be67e4674961fafab11596e181a18cdc88bf4b35c4ebdc16b6d54e024 /var/run/docker/libcontainerd/146ec82be67e4674961fafab11596e181a18cdc88bf4b35c4ebdc16b6d54e024 docker-runc
  │   │   │   ├─kube-proxy,2680 --config=/var/lib/kube-proxy/config.conf
  │   │   │   │   ├─{kube-proxy},2707
  │   │   │   │   ├─{kube-proxy},2708
  │   │   │   │   ├─{kube-proxy},2709
  │   │   │   │   ├─{kube-proxy},2711
  │   │   │   │   └─{kube-proxy},2734
  │   │   │   ├─{docker-containe},2668
  │   │   │   ├─{docker-containe},2669
  │   │   │   ├─{docker-containe},2670
  │   │   │   ├─{docker-containe},2671
  │   │   │   ├─{docker-containe},2672
  │   │   │   ├─{docker-containe},2693
  │   │   │   ├─{docker-containe},2695
  │   │   │   └─{docker-containe},2696
  │   │   ├─docker-containe,3840 33f15a0a937f9e16a9728306e6cab7141675c65f13a0e55c6ade102e36ecab48 /var/run/docker/libcontainerd/33f15a0a937f9e16a9728306e6cab7141675c65f13a0e55c6ade102e36ecab48 docker-runc
  │   │   │   ├─pause,3853
  │   │   │   ├─{docker-containe},3841
  │   │   │   ├─{docker-containe},3842
  │   │   │   ├─{docker-containe},3843
  │   │   │   ├─{docker-containe},3844
  │   │   │   ├─{docker-containe},3845
  │   │   │   ├─{docker-containe},3858
  │   │   │   ├─{docker-containe},3859
  │   │   │   └─{docker-containe},3860
  │   │   ├─{docker-containe},1281
  │   │   ├─{docker-containe},1282
  │   │   ├─{docker-containe},1283
  │   │   ├─{docker-containe},1284
  │   │   ├─{docker-containe},1285
  │   │   ├─{docker-containe},1286
  │   │   ├─{docker-containe},2207
  │   │   ├─{docker-containe},2317
  │   │   ├─{docker-containe},2324
  │   │   ├─{docker-containe},2325
  │   │   ├─{docker-containe},2332
  │   │   ├─{docker-containe},2333
  │   │   ├─{docker-containe},2340
  │   │   ├─{docker-containe},2341
  │   │   ├─{docker-containe},2342
  │   │   ├─{docker-containe},2343
  │   │   ├─{docker-containe},2344
  │   │   └─{docker-containe},2345
  │   ├─{dockerd},1185
  │   ├─{dockerd},1186
  │   ├─{dockerd},1264
  │   ├─{dockerd},1272
  │   ├─{dockerd},1273
  │   ├─{dockerd},1287
  │   ├─{dockerd},1288
  │   ├─{dockerd},1289
  │   ├─{dockerd},1439
  │   ├─{dockerd},2193
  │   ├─{dockerd},2202
  │   ├─{dockerd},2203
  │   ├─{dockerd},2204
  │   ├─{dockerd},2242
  │   ├─{dockerd},2243
  │   ├─{dockerd},2357
  │   ├─{dockerd},2358
  │   ├─{dockerd},2359
  │   ├─{dockerd},2372
  │   ├─{dockerd},2403
  │   ├─{dockerd},2405
  │   ├─{dockerd},2447
  │   ├─{dockerd},2452
  │   ├─{dockerd},2453
  │   ├─{dockerd},2521
  │   ├─{dockerd},2665
  │   ├─{dockerd},2666
  │   ├─{dockerd},3838
  │   ├─{dockerd},3839
  │   └─{dockerd},5433
  ├─iscsid,1170
  ├─iscsid,1171
  ├─kubelet,2120 --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml --cgroup-driver=cgroupfs --network-plugin=cni
  │   ├─{kubelet},2123
  │   ├─{kubelet},2124
  │   ├─{kubelet},2125
  │   ├─{kubelet},2126
  │   ├─{kubelet},2127
  │   ├─{kubelet},2128
  │   ├─{kubelet},2146
  │   ├─{kubelet},2147
  │   ├─{kubelet},2177
  │   ├─{kubelet},2190
  │   ├─{kubelet},2191
  │   └─{kubelet},2192

```

## 安装 Pod 网络

要让 Kubernetes Cluster 能够工作，必须安装 Pod 网络，否则 Pod 之间无法通信。

Kubernetes 支持多种网络方案，这里我们先使用 flannel，后面还会讨论 Canal。

执行如下命令部署 flannel：

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

```
## 在k8s-node 机器上执行kubeadm  join 加入集群。

添加 k8s-node1 和 k8s-node2
在 k8s-node1 和 k8s-node2 上分别执行如下命令，将其注册到 Cluster 中：
```
root@k8s-node1:~# kubeadm join 10.0.63.47:6443 --token s56myc.82qpolpdadevbt8r --discovery-token-ca-cert-hash sha256:b075883d2963b624cfe8fe86ac9f7992724dc1af9fb912e1d637481722e6ccaf
[preflight] running pre-flight checks
	[WARNING RequiredIPVSKernelModulesAvailable]: the IPVS proxier will not be used, because the following required kernel modules are not loaded: [ip_vs ip_vs_rr ip_vs_wrr ip_vs_sh] or no builtin kernel ipvs support: map[nf_conntrack_ipv4:{} ip_vs:{} ip_vs_rr:{} ip_vs_wrr:{} ip_vs_sh:{}]
you can solve this problem with following methods:
 1. Run 'modprobe -- ' to load missing kernel modules;
2. Provide the missing builtin kernel ipvs support

[discovery] Trying to connect to API Server "10.0.63.47:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://10.0.63.47:6443"
[discovery] Requesting info from "https://10.0.63.47:6443" again to validate TLS against the pinned public key
[discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "10.0.63.47:6443"
[discovery] Successfully established connection with API Server "10.0.63.47:6443"
[kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.12" ConfigMap in the kube-system namespace
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[preflight] Activating the kubelet service
[tlsbootstrap] Waiting for the kubelet to perform the TLS Bootstrap...
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "k8s-node1" as an annotation

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the master to see this node join the cluster.

root@k8s-node1:~# kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
root@k8s-node1:~# 
```


/etc/kubernets/下面会创建 bootstrap-kubelet.conf和kubelet.conf文件，和目录pki/下的ca.crt文件。

```
root@k8s-node2:/etc/kubernetes# cat bootstrap-kubelet.conf 
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNE1URXlNakExTlRZd01Wb1hEVEk0TVRFeE9UQTFOVFl3TVZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBSzlkClkxajVyY2Roa25GRDlFeGYrUGtlYzBQRTNUT1hSakZUelozRmZpZWhJcmt3ejJNbWY1UnQvdjlUekdrQjNlNXoKVExRMlUxYWNzZ2czNnhhVm81RWpXcFhUc3ptZXVLWFZxQXdRRURHbHcrdmo2U3F6MUV2Umw1YXpUejBOWkUxaQpadUlwTFJzU1RGdmVCUy9QdmgwSXZaRjdKcldTU3lLakFaWkZkS0lNOUJ2Rnc5cGFoNi9KUHZ2NWVLZW83MWhXCjY5SHg1bVhGdUc0SzFHYlVZVFk5T25USDhKV0N6aUpRUWFyVUNITmloYXZyUG40YzlPZlZTS1NNNXVYR0Q1R2wKcTN1Nm4rOUcrdlp0N3YwKzdNSS95ZFRRZTV2WVB5N0ZoRUYwY1JablFJZkF0cG5YeUxhd0ZkVGpMOThjTUp6dQp4bWlnam9rS3licTVJbGM3dUU4Q0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFJSEJaVUQ0bzV0OVdYdDR5a3k5akJNelNONngKcFNpK2JzOUNyTHE3UmFsbVRCelFkVWUvWVU2Zng3Z0lRRkRhRU4yNGowdlM3eWUvb1Q2K2QwRW5KZDE2S1p4SApMMEhlbXZkbFVzbVJvV3FLR0xsYXhuQi96NEJYemZzRnN6OEhUcDZsaUtwSlFvMDZZdlNsaHJmMmE5UFdPQ3k0CnRYb1ZEcXQwb1VqakNwNWxaNFlSbERNSk9xRDRZTWNFdVJabmI5Tnh0a2tXQnU2VDE1VkVpMEtoRnRPdXE3Y2YKY1VkV0VId1lmWklab0VFWlI4cEtYTmNzUzkwOXhKVndOQ3VoNC9RMWlubGVsQ3d6TFVnZkZpc0IwWDFhYVFOWQpkdi9sdU9tRmh2S3NoWmZMUm8zKzhWTzl6cVpZS0d0YkVnWURQSDJoTnlwKzRUNmJHckxtbTVaUjVuYz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://10.0.63.47:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: tls-bootstrap-token-user
  name: tls-bootstrap-token-user@kubernetes
current-context: tls-bootstrap-token-user@kubernetes
kind: Config
preferences: {}
users:
- name: tls-bootstrap-token-user
  user:
    token: s56myc.82qpolpdadevbt8r
root@k8s-node2:/etc/kubernetes# 
```

```
root@k8s-node2:/etc/kubernetes# cat kubelet.conf 
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNE1URXlNakExTlRZd01Wb1hEVEk0TVRFeE9UQTFOVFl3TVZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBSzlkClkxajVyY2Roa25GRDlFeGYrUGtlYzBQRTNUT1hSakZUelozRmZpZWhJcmt3ejJNbWY1UnQvdjlUekdrQjNlNXoKVExRMlUxYWNzZ2czNnhhVm81RWpXcFhUc3ptZXVLWFZxQXdRRURHbHcrdmo2U3F6MUV2Umw1YXpUejBOWkUxaQpadUlwTFJzU1RGdmVCUy9QdmgwSXZaRjdKcldTU3lLakFaWkZkS0lNOUJ2Rnc5cGFoNi9KUHZ2NWVLZW83MWhXCjY5SHg1bVhGdUc0SzFHYlVZVFk5T25USDhKV0N6aUpRUWFyVUNITmloYXZyUG40YzlPZlZTS1NNNXVYR0Q1R2wKcTN1Nm4rOUcrdlp0N3YwKzdNSS95ZFRRZTV2WVB5N0ZoRUYwY1JablFJZkF0cG5YeUxhd0ZkVGpMOThjTUp6dQp4bWlnam9rS3licTVJbGM3dUU4Q0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFJSEJaVUQ0bzV0OVdYdDR5a3k5akJNelNONngKcFNpK2JzOUNyTHE3UmFsbVRCelFkVWUvWVU2Zng3Z0lRRkRhRU4yNGowdlM3eWUvb1Q2K2QwRW5KZDE2S1p4SApMMEhlbXZkbFVzbVJvV3FLR0xsYXhuQi96NEJYemZzRnN6OEhUcDZsaUtwSlFvMDZZdlNsaHJmMmE5UFdPQ3k0CnRYb1ZEcXQwb1VqakNwNWxaNFlSbERNSk9xRDRZTWNFdVJabmI5Tnh0a2tXQnU2VDE1VkVpMEtoRnRPdXE3Y2YKY1VkV0VId1lmWklab0VFWlI4cEtYTmNzUzkwOXhKVndOQ3VoNC9RMWlubGVsQ3d6TFVnZkZpc0IwWDFhYVFOWQpkdi9sdU9tRmh2S3NoWmZMUm8zKzhWTzl6cVpZS0d0YkVnWURQSDJoTnlwKzRUNmJHckxtbTVaUjVuYz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://10.0.63.47:6443
  name: default-cluster
contexts:
- context:
    cluster: default-cluster
    namespace: default
    user: default-auth
  name: default-context
current-context: default-context
kind: Config
preferences: {}
users:
- name: default-auth
  user:
    client-certificate: /var/lib/kubelet/pki/kubelet-client-current.pem
    client-key: /var/lib/kubelet/pki/kubelet-client-current.pem
root@k8s-node2:/etc/kubernetes#
```

```
root@k8s-node2:/etc/kubernetes# cat pki/ca.crt 
-----BEGIN CERTIFICATE-----
MIICyDCCAbCgAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
cm5ldGVzMB4XDTE4MTEyMjA1NTYwMVoXDTI4MTExOTA1NTYwMVowFTETMBEGA1UE
AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAK9d
Y1j5rcdhknFD9Exf+Pkec0PE3TOXRjFTzZ3FfiehIrkwz2Mmf5Rt/v9TzGkB3e5z
TLQ2U1acsgg36xaVo5EjWpXTszmeuKXVqAwQEDGlw+vj6Sqz1EvRl5azTz0NZE1i
ZuIpLRsSTFveBS/Pvh0IvZF7JrWSSyKjAZZFdKIM9BvFw9pah6/JPvv5eKeo71hW
69Hx5mXFuG4K1GbUYTY9OnTH8JWCziJQQarUCHNihavrPn4c9OfVSKSM5uXGD5Gl
q3u6n+9G+vZt7v0+7MI/ydTQe5vYPy7FhEF0cRZnQIfAtpnXyLawFdTjL98cMJzu
xmigjokKybq5Ilc7uE8CAwEAAaMjMCEwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
/wQFMAMBAf8wDQYJKoZIhvcNAQELBQADggEBAIHBZUD4o5t9WXt4yky9jBMzSN6x
pSi+bs9CrLq7RalmTBzQdUe/YU6fx7gIQFDaEN24j0vS7ye/oT6+d0EnJd16KZxH
L0HemvdlUsmRoWqKGLlaxnB/z4BXzfsFsz8HTp6liKpJQo06YvSlhrf2a9PWOCy4
tXoVDqt0oUjjCp5lZ4YRlDMJOqD4YMcEuRZnb9NxtkkWBu6T15VEi0KhFtOuq7cf
cUdWEHwYfZIZoEEZR8pKXNcsS909xJVwNCuh4/Q1inlelCwzLUgfFisB0X1aaQNY
dv/luOmFhvKshZfLRo3+8VO9zqZYKGtbEgYDPH2hNyp+4T6bGrLmm5ZR5nc=
-----END CERTIFICATE-----
root@k8s-node2:/etc/kubernetes#
```

如果这3个文件存在，可以删除了。重新是有 join 加入，不然会报下面的错误的。我刚开始就是被这个折腾了好久。
```
root@k8s-node2:/etc/kubernetes# kubeadm join 10.0.63.47:6443 --token s56myc.82qpolpdadevbt8r --discovery-token-ca-cert-hash sha256:b075883d2963b624cfe8fe86ac9f7992724dc1af9fb912e1d637481722e6ccaf
[preflight] running pre-flight checks
[preflight] Some fatal errors occurred:
	[ERROR FileAvailable--etc-kubernetes-kubelet.conf]: /etc/kubernetes/kubelet.conf already exists
	[ERROR FileAvailable--etc-kubernetes-bootstrap-kubelet.conf]: /etc/kubernetes/bootstrap-kubelet.conf already exists
	[ERROR Port-10250]: Port 10250 is in use
	[ERROR FileAvailable--etc-kubernetes-pki-ca.crt]: /etc/kubernetes/pki/ca.crt already exists
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
root@k8s-node2:/etc/kubernetes# 
```







```
systemd,1
  ├─accounts-daemon,872
  │   ├─{gdbus},915
  │   └─{gmain},913
  ├─acpid,871
  ├─agetty,1242 --noclear tty1 linux
  ├─atd,883 -f
  ├─cron,857 -f
  ├─dbus-daemon,894 --system --address=systemd: --nofork --nopidfile --systemd-activation
  ├─dhclient,977 -1 -v -pf /run/dhclient.enp0s3.pid -lf /var/lib/dhcp/dhclient.enp0s3.leases -I -df /var/lib/dhcp/dhclient6.enp0s3.leases enp0s3
  ├─dockerd,1166 -H fd://
  │   ├─docker-containe,1277 -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --metrics-interval=0 --start-timeout 2m --state-dir /var/run/docker/libcontainerd/containerd --shim docker-containerd-shim --runtime docker-runc
  │   │   ├─docker-containe,2375 c77df2152263be2a28b0e00eb7a31161f7e15fa5f29d0c7b8547d16e6337c86e /var/run/docker/libcontainerd/c77df2152263be2a28b0e00eb7a31161f7e15fa5f29d0c7b8547d16e6337c86e docker-runc
  │   │   │   ├─pause,2409
  │   │   │   ├─{docker-containe},2376
  │   │   │   ├─{docker-containe},2377
  │   │   │   ├─{docker-containe},2378
  │   │   │   ├─{docker-containe},2379
  │   │   │   ├─{docker-containe},2380
  │   │   │   ├─{docker-containe},2386
  │   │   │   ├─{docker-containe},2387
  │   │   │   └─{docker-containe},2388
  │   │   ├─docker-containe,2391 e2406d3bb477906e81ecb9dc5886da16c6bd111a40048134644db78f5aecbb23 /var/run/docker/libcontainerd/e2406d3bb477906e81ecb9dc5886da16c6bd111a40048134644db78f5aecbb23 docker-runc
  │   │   │   ├─pause,2422
  │   │   │   ├─{docker-containe},2392
  │   │   │   ├─{docker-containe},2393
  │   │   │   ├─{docker-containe},2394
  │   │   │   ├─{docker-containe},2395
  │   │   │   ├─{docker-containe},2396
  │   │   │   ├─{docker-containe},2402
  │   │   │   ├─{docker-containe},2403
  │   │   │   └─{docker-containe},2404
  │   │   ├─docker-containe,2480 c5b2b76b727ed81e0c0ef1bdbe07f33ac7ef5bebde4a18280c20dd0aff13c15f /var/run/docker/libcontainerd/c5b2b76b727ed81e0c0ef1bdbe07f33ac7ef5bebde4a18280c20dd0aff13c15f docker-runc
  │   │   │   ├─kube-proxy,2493 --config=/var/lib/kube-proxy/config.conf
  │   │   │   │   ├─{kube-proxy},2520
  │   │   │   │   ├─{kube-proxy},2521
  │   │   │   │   ├─{kube-proxy},2522
  │   │   │   │   ├─{kube-proxy},2524
  │   │   │   │   └─{kube-proxy},2529
  │   │   │   ├─{docker-containe},2481
  │   │   │   ├─{docker-containe},2482
  │   │   │   ├─{docker-containe},2483
  │   │   │   ├─{docker-containe},2484
  │   │   │   ├─{docker-containe},2485
  │   │   │   ├─{docker-containe},2506
  │   │   │   ├─{docker-containe},2508
  │   │   │   └─{docker-containe},2509
  │   │   ├─{docker-containe},1278
  │   │   ├─{docker-containe},1279
  │   │   ├─{docker-containe},1280
  │   │   ├─{docker-containe},1281
  │   │   ├─{docker-containe},1282
  │   │   ├─{docker-containe},1283
  │   │   ├─{docker-containe},1660
  │   │   ├─{docker-containe},2454
  │   │   ├─{docker-containe},2461
  │   │   ├─{docker-containe},2462
  │   │   ├─{docker-containe},2463
  │   │   └─{docker-containe},2464
  │   ├─{dockerd},1258
  │   ├─{dockerd},1260
  │   ├─{dockerd},1267
  │   ├─{dockerd},1269
  │   ├─{dockerd},1270
  │   ├─{dockerd},1288
  │   ├─{dockerd},1289
  │   ├─{dockerd},1293
  │   ├─{dockerd},1437
  │   ├─{dockerd},2389
  │   ├─{dockerd},2390
  │   ├─{dockerd},2472
  │   └─{dockerd},2479
  ├─iscsid,1183
  ├─iscsid,1184
  ├─kubelet,2288 --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml --cgroup-driver=cgroupfs --network-plugin=cni
  │   ├─{kubelet},2289
  │   ├─{kubelet},2290
  │   ├─{kubelet},2292
  │   ├─{kubelet},2294
  │   ├─{kubelet},2295
  │   ├─{kubelet},2299
  │   ├─{kubelet},2313
  │   ├─{kubelet},2328
  │   ├─{kubelet},2353
  │   ├─{kubelet},2354
  │   ├─{kubelet},2355
  │   └─{kubelet},2356

```
如果加入成功了，可以在master节点执行  kubectl get nodes 验证

```
mamh@k8s-master:~$ kubectl get nodes
NAME         STATUS     ROLES    AGE   VERSION
k8s-master   NotReady   master   32m   v1.12.2
k8s-node1    NotReady   <none>   26m   v1.12.2
k8s-node2    NotReady   <none>   12m   v1.12.2

```
目前所有节点都是 NotReady，这是因为每个节点都需要启动若干组件，这些组件都是在 Pod 中运行，
需要首先从 google 下载镜像，我们可以通过如下命令查看 Pod 的状态：kubectl get pod --all-namespaces

```
mamh@k8s-master:~$ kubectl get pod --all-namespaces
NAMESPACE     NAME                                 READY   STATUS                  RESTARTS   AGE
kube-system   coredns-576cbf47c7-278z7             0/1     ContainerCreating       0          60m
kube-system   coredns-576cbf47c7-zt762             0/1     ContainerCreating       0          60m
kube-system   etcd-k8s-master                      1/1     Running                 2          54m
kube-system   kube-apiserver-k8s-master            1/1     Running                 4          54m
kube-system   kube-controller-manager-k8s-master   1/1     Running                 2          54m
kube-system   kube-flannel-ds-amd64-hcb4x          0/1     Init:ImagePullBackOff   0          40m
kube-system   kube-flannel-ds-amd64-jg9x8          0/1     Init:ImagePullBackOff   0          54m
kube-system   kube-flannel-ds-amd64-ld8g5          0/1     Init:ImagePullBackOff   0          55m
kube-system   kube-proxy-s74kv                     1/1     Running                 1          60m
kube-system   kube-proxy-vgw9q                     1/1     Running                 1          40m
kube-system   kube-proxy-x95m6                     1/1     Running                 1          54m
kube-system   kube-scheduler-k8s-master            1/1     Running                 2          54m
mamh@k8s-master:~$ 

```
Pending、ContainerCreating、ImagePullBackOff 都表明 Pod 没有就绪，Running 才是就绪状态。我们可以通过 kubectl describe pod <Pod Name> 查看 Pod 具体情况，比如：

```
mamh@k8s-master:~$ kubectl describe pod kube-flannel-ds-amd64-ld8g5 --namespace=kube-system
Name:               kube-flannel-ds-amd64-ld8g5
Namespace:          kube-system
Priority:           0
PriorityClassName:  <none>
Node:               k8s-master/10.0.63.47
Start Time:         Wed, 21 Nov 2018 22:01:45 -0800
Labels:             app=flannel
                    controller-revision-hash=6697bf5fc6
                    pod-template-generation=1
                    tier=node
Annotations:        <none>
Status:             Pending
IP:                 10.0.63.47
Controlled By:      DaemonSet/kube-flannel-ds-amd64
Init Containers:
  install-cni:
    Container ID:  
    Image:         quay.io/coreos/flannel:v0.10.0-amd64
    Image ID:      
    Port:          <none>
    Host Port:     <none>
    Command:
      cp
    Args:
      -f
      /etc/kube-flannel/cni-conf.json
      /etc/cni/net.d/10-flannel.conflist
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /etc/cni/net.d from cni (rw)
      /etc/kube-flannel/ from flannel-cfg (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from flannel-token-xrggc (ro)
Containers:
  kube-flannel:
    Container ID:  
    Image:         quay.io/coreos/flannel:v0.10.0-amd64
    Image ID:      
    Port:          <none>
    Host Port:     <none>
    Command:
      /opt/bin/flanneld
    Args:
      --ip-masq
      --kube-subnet-mgr
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  50Mi
    Requests:
      cpu:     100m
      memory:  50Mi
    Environment:
      POD_NAME:       kube-flannel-ds-amd64-ld8g5 (v1:metadata.name)
      POD_NAMESPACE:  kube-system (v1:metadata.namespace)
    Mounts:
      /etc/kube-flannel/ from flannel-cfg (rw)
      /run from run (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from flannel-token-xrggc (ro)
Conditions:
  Type              Status
  Initialized       False 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  run:
    Type:          HostPath (bare host directory volume)
    Path:          /run
    HostPathType:  
  cni:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/cni/net.d
    HostPathType:  
  flannel-cfg:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      kube-flannel-cfg
    Optional:  false
  flannel-token-xrggc:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  flannel-token-xrggc
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  beta.kubernetes.io/arch=amd64
Tolerations:     :NoSchedule
                 node.kubernetes.io/disk-pressure:NoSchedule
                 node.kubernetes.io/memory-pressure:NoSchedule
                 node.kubernetes.io/network-unavailable:NoSchedule
                 node.kubernetes.io/not-ready:NoExecute
                 node.kubernetes.io/unreachable:NoExecute
                 node.kubernetes.io/unschedulable:NoSchedule
Events:
  Type     Reason     Age                  From                 Message
  ----     ------     ----                 ----                 -------
  Normal   Scheduled  55m                  default-scheduler    Successfully assigned kube-system/kube-flannel-ds-amd64-ld8g5 to k8s-master
  Warning  Failed     55m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:44628->23.23.143.106:443: read: connection reset by peer
  Warning  Failed     55m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:44640->23.23.143.106:443: read: connection reset by peer
  Warning  Failed     55m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:44664->23.23.143.106:443: read: connection reset by peer
  Normal   Pulling    54m (x4 over 55m)    kubelet, k8s-master  pulling image "quay.io/coreos/flannel:v0.10.0-amd64"
  Warning  Failed     54m (x4 over 55m)    kubelet, k8s-master  Error: ErrImagePull
  Warning  Failed     54m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:42900->23.21.58.63:443: read: connection reset by peer
  Warning  Failed     30m (x108 over 55m)  kubelet, k8s-master  Error: ImagePullBackOff
  Normal   BackOff    25m (x132 over 55m)  kubelet, k8s-master  Back-off pulling image "quay.io/coreos/flannel:v0.10.0-amd64"
  Warning  Failed     15m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:45092->54.243.169.123:443: read: connection reset by peer
  Warning  Failed     15m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:45106->54.243.169.123:443: read: connection reset by peer
  Warning  Failed     15m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:45136->54.243.169.123:443: read: connection reset by peer
  Warning  Failed     14m (x4 over 15m)    kubelet, k8s-master  Error: ErrImagePull
  Normal   Pulling    14m (x4 over 15m)    kubelet, k8s-master  pulling image "quay.io/coreos/flannel:v0.10.0-amd64"
  Warning  Failed     14m                  kubelet, k8s-master  Failed to pull image "quay.io/coreos/flannel:v0.10.0-amd64": rpc error: code = Unknown desc = Error response from daemon: Get https://quay.io/v1/_ping: read tcp 10.0.63.47:45180->54.243.169.123:443: read: connection reset by peer
  Normal   BackOff    14m (x6 over 15m)    kubelet, k8s-master  Back-off pulling image "quay.io/coreos/flannel:v0.10.0-amd64"
  Warning  Failed     28s (x65 over 15m)   kubelet, k8s-master  Error: ImagePullBackOff
mamh@k8s-master:~$ 
```
通过错误提示我们发现是有个image没有下载。这个估计也是要访问国外的。

```
docker pull registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64
docker tag registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64


```

```
mamh@k8s-master:~$ kubectl describe pod coredns-576cbf47c7-zt762 --namespace=kube-system
Name:               coredns-576cbf47c7-zt762
Namespace:          kube-system
Priority:           0
PriorityClassName:  <none>
Node:               k8s-node1/10.0.63.53
Start Time:         Wed, 21 Nov 2018 22:02:45 -0800
Labels:             k8s-app=kube-dns
                    pod-template-hash=576cbf47c7
Annotations:        <none>
Status:             Pending
IP:                 
Controlled By:      ReplicaSet/coredns-576cbf47c7
Containers:
  coredns:
    Container ID:  
    Image:         k8s.gcr.io/coredns:1.2.2
    Image ID:      
    Ports:         53/UDP, 53/TCP, 9153/TCP
    Host Ports:    0/UDP, 0/TCP, 0/TCP
    Args:
      -conf
      /etc/coredns/Corefile
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Limits:
      memory:  170Mi
    Requests:
      cpu:        100m
      memory:     70Mi
    Liveness:     http-get http://:8080/health delay=60s timeout=5s period=10s #success=1 #failure=5
    Environment:  <none>
    Mounts:
      /etc/coredns from config-volume (ro)
      /var/run/secrets/kubernetes.io/serviceaccount from coredns-token-xb2rr (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  config-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      coredns
    Optional:  false
  coredns-token-xb2rr:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  coredns-token-xb2rr
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     CriticalAddonsOnly
                 node-role.kubernetes.io/master:NoSchedule
                 node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                  From                Message
  ----     ------            ----                 ----                -------
  Warning  FailedScheduling  56m (x32 over 61m)   default-scheduler   0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Warning  NetworkNotReady   25m (x141 over 55m)  kubelet, k8s-node1  network is not ready: [runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized]
  Warning  NetworkNotReady   82s (x73 over 16m)   kubelet, k8s-node1  network is not ready: [runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized]
mamh@k8s-master:~$ 

```
等待一段时间，image 都成功下载后，所有 Pod 会处于 Running 状态。


```
mamh@k8s-master:~$ kubectl get pod --all-namespaces
NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
kube-system   coredns-576cbf47c7-278z7             1/1     Running   0          75m
kube-system   coredns-576cbf47c7-zt762             1/1     Running   0          75m
kube-system   etcd-k8s-master                      1/1     Running   2          69m
kube-system   kube-apiserver-k8s-master            1/1     Running   4          69m
kube-system   kube-controller-manager-k8s-master   1/1     Running   2          69m
kube-system   kube-flannel-ds-amd64-hcb4x          1/1     Running   0          55m
kube-system   kube-flannel-ds-amd64-jg9x8          1/1     Running   0          69m
kube-system   kube-flannel-ds-amd64-ld8g5          1/1     Running   0          70m
kube-system   kube-proxy-s74kv                     1/1     Running   1          75m
kube-system   kube-proxy-vgw9q                     1/1     Running   1          55m
kube-system   kube-proxy-x95m6                     1/1     Running   1          69m
kube-system   kube-scheduler-k8s-master            1/1     Running   2          69m

```
这时，所有的节点都已经 Ready，Kubernetes Cluster 创建成功，一切准备就绪。

```
mamh@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE   VERSION
k8s-master   Ready    master   76m   v1.12.2
k8s-node1    Ready    <none>   70m   v1.12.2
k8s-node2    Ready    <none>   56m   v1.12.2
```
## 小结

本章通过 kubeadm 部署了三节点的 Kubernetes 集群，后面章节我们都将在这个实验环境中学习 Kubernetes 的各项技术。


Kubernetes Cluster 由 Master 和 Node 组成，节点上运行着若干 Kubernetes 服务。
Master 节点
Master 是 Kubernetes Cluster 的大脑，运行着如下 Daemon 服务：kube-apiserver、kube-scheduler、kube-controller-manager、etcd 和 Pod 网络（例如 flannel）。

```
API Server（kube-apiserver）
API Server 提供 HTTP/HTTPS RESTful API，即 Kubernetes API。API Server 是 Kubernetes Cluster 的前端接口，各种客户端工具（CLI 或 UI）以及 Kubernetes 其他组件可以通过它管理 Cluster 的各种资源。
  |   |   |   |-kube-apiserver,1613 --authorization-mode=Node,RBAC --advertise-address=10.0.63.47 --allow-privileged=true --client-ca-file=/etc/kubernetes/pki/ca.crt --enable-admission-plugins=NodeRestriction --enable-bootstrap-token-auth=true --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key --etcd-servers=https://127.0.0.1:2379 --insecure-port=0 --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key --requestheader-allowed-names=front-proxy-client --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt --requestheader-extra-headers-prefix=X-Remote-Extra- --requestheader-group-headers=X-Remote-Group --requestheader-username-headers=X-Remote-User --secure-port=6443 --service-account-key-file=/etc/kubernetes/pki/sa.pub --service-cluster-ip-range=10.96.0.0/12 --tls-cert-file=/etc/kubernetes/pki/apiserver.crt --tls-private-key-file=/etc/kubernetes/pki/apiserver.key

Scheduler（kube-scheduler）
Scheduler 负责决定将 Pod 放在哪个 Node 上运行。Scheduler 在调度时会充分考虑 Cluster 的拓扑结构，当前各个节点的负载，以及应用对高可用、性能、数据亲和性的需求。

Controller Manager（kube-controller-manager）
Controller Manager 负责管理 Cluster 各种资源，保证资源处于预期的状态。Controller Manager 由多种 controller 组成，包括 replication controller、endpoints controller、namespace controller、serviceaccounts controller 等。
不同的 controller 管理不同的资源。例如 replication controller 管理 Deployment、StatefulSet、DaemonSet 的生命周期，namespace controller 管理 Namespace 资源。
  |   |   |   |-kube-controller,1878 --address=127.0.0.1 --allocate-node-cidrs=true --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf --authorization-kubeconfig=/etc/kubernetes/controller-manager.conf --client-ca-file=/etc/kubernetes/pki/ca.crt --cluster-cidr=10.244.0.0/16 --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt --cluster-signing-key-file=/etc/kubernetes/pki/ca.key --controllers=*,bootstrapsigner,tokencleaner --kubeconfig=/etc/kubernetes/controller-manager.conf --leader-elect=true --node-cidr-mask-size=24 --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt --root-ca-file=/etc/kubernetes/pki/ca.crt --service-account-private-key-file=/etc/kubernetes/pki/sa.key --use-service-account-credentials=true

etcd
etcd 负责保存 Kubernetes Cluster 的配置信息和各种资源的状态信息。当数据发生变化时，etcd 会快速地通知 Kubernetes 相关组件。
  |   |   |   |-etcd,1843 --advertise-client-urls=https://127.0.0.1:2379 --cert-file=/etc/kubernetes/pki/etcd/server.crt --client-cert-auth=true --data-dir=/var/lib/etcd --initial-advertise-peer-urls=https://127.0.0.1:2380 --initial-cluster=k8s-master=https://127.0.0.1:2380 --key-file=/etc/kubernetes/pki/etcd/server.key --listen-client-urls=https://127.0.0.1:2379 --listen-peer-urls=https://127.0.0.1:2380 --name=k8s-master --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt --peer-client-cert-auth=true --peer-key-file=/etc/kubernetes/pki/etcd/peer.key --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt --snapshot-count=10000 --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt

Pod 网络
Pod 要能够相互通信，Kubernetes Cluster 必须部署 Pod 网络，flannel 是其中一个可选方案。

```

## 所需脚本
install-k8s.sh
```bash
#!/bin/bash

apt-get update && apt-get install docker.io


apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 

cat << EOF > /etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF

apt-get update
apt-get install -y kubelet kubeadm kubectl

adduser buildfarm docker
```

install-images.sh
```bash

#/bin/bash


docker pull mirrorgooglecontainers/kube-apiserver:v1.12.2
docker pull mirrorgooglecontainers/kube-controller-manager:v1.12.2
docker pull mirrorgooglecontainers/kube-scheduler:v1.12.2
docker pull mirrorgooglecontainers/kube-proxy:v1.12.2
docker pull mirrorgooglecontainers/pause:3.1
docker pull mirrorgooglecontainers/etcd:3.2.24
docker pull kuberneter/coredns:1.2.2
docker pull registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64


docker tag mirrorgooglecontainers/kube-apiserver:v1.12.2 k8s.gcr.io/kube-apiserver:v1.12.2
docker tag mirrorgooglecontainers/kube-controller-manager:v1.12.2 k8s.gcr.io/kube-controller-manager:v1.12.2
docker tag mirrorgooglecontainers/kube-scheduler:v1.12.2 k8s.gcr.io/kube-scheduler:v1.12.2
docker tag mirrorgooglecontainers/kube-proxy:v1.12.2 k8s.gcr.io/kube-proxy:v1.12.2
docker tag mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.1
docker tag mirrorgooglecontainers/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24
docker tag kuberneter/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2

####################################################################################################################################
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-proxy:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-apiserver:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-controller-manager:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-scheduler:v1.12.3 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/etcd:3.2.24 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/coredns:1.2.2 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/flannel:v0.10.0-amd64 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/pause:3.1 ;\
docker pull registry.cn-hangzhou.aliyuncs.com/kuberimages/kubernetes-dashboard-amd64:v1.10.0



docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-proxy:v1.12.3 k8s.gcr.io/kube-proxy:v1.12.3 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-apiserver:v1.12.3 k8s.gcr.io/kube-apiserver:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-controller-manager:v1.12.3 k8s.gcr.io/kube-controller-manager:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kube-scheduler:v1.12.3 k8s.gcr.io/kube-scheduler:v1.12.3;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/coredns:1.2.2 k8s.gcr.io/coredns:1.2.2 ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64   ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/pause:3.1 k8s.gcr.io/pause:3.1  ;\
docker tag registry.cn-hangzhou.aliyuncs.com/kuberimages/kubernetes-dashboard-amd64:v1.10.0 k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.0

#gcr.io/kubernetes-helm/tiller:v2.11.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.11.0
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.11.0 gcr.io/kubernetes-helm/tiller:v2.11.0

#quay.io/calico/node:v3.3.2
docker pull registry.cn-hangzhou.aliyuncs.com/liuq/calico-node:v2.6.2
docker tag  registry.cn-hangzhou.aliyuncs.com/liuq/calico-node:v2.6.2 quay.io/calico/node:v2.6.2

# k8s.gcr.io/heapster-amd64:v1.5.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4 k8s.gcr.io/heapster-amd64:v1.5.4

# k8s.gcr.io/heapster-grafana-amd64:v5.0.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4 k8s.gcr.io/heapster-grafana-amd64:v5.0.4

# k8s.gcr.io/heapster-influxdb-amd64:v1.5.2
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2
docker tag  registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2 k8s.gcr.io/heapster-influxdb-amd64:v1.5.2

```
kubeadm-init.sh
```bash
#!/bin/bash

kubeadm init --apiserver-advertise-address 10.0.12.62 --pod-network-cidr=10.244.0.0/16 --kubernetes-version=v1.12.3
```
kubeadm-join.sh
```bash

#!/bin/bash

kubeadm join 10.0.12.62:6443 --token 6mltlc.w0gkos3agrjw1o7m --discovery-token-ca-cert-hash sha256:02500666dae4977b96032d5ba287bc68c0ee3654076b30e119a0de7079f5618a

```












