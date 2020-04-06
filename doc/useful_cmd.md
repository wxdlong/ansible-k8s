

1. Check K8s images version list `kubeadm config images list`.

```bash
[root@wxd ~]# kubeadm config images list
I0406 11:15:23.195018   12815 version.go:251] remote version is much newer: v1.18.0; falling back to: stable-1.17
W0406 11:15:25.808636   12815 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0406 11:15:25.808651   12815 validation.go:28] Cannot validate kubelet config - no validator is available
k8s.gcr.io/kube-apiserver:v1.17.4
k8s.gcr.io/kube-controller-manager:v1.17.4
k8s.gcr.io/kube-scheduler:v1.17.4
k8s.gcr.io/kube-proxy:v1.17.4
k8s.gcr.io/pause:3.1
k8s.gcr.io/etcd:3.4.3-0
k8s.gcr.io/coredns:1.6.5

```

2. Print init config `kubeadm config print init-defaults`

```bash
[root@wxd ~]# kubeadm config print init-defaults
W0406 11:16:56.665525   12865 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0406 11:16:56.665579   12865 validation.go:28] Cannot validate kubelet config - no validator is available
apiVersion: kubeadm.k8s.io/v1beta2
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 1.2.3.4
  bindPort: 6443
nodeRegistration:
  criSocket: /var/run/dockershim.sock
  name: wxd.long
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta2
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: k8s.gcr.io
kind: ClusterConfiguration
kubernetesVersion: v1.17.0
networking:
  dnsDomain: cluster.local
  serviceSubnet: 10.96.0.0/12
scheduler: {}

```

3. describe pod ``


4. check ipvs list `ipvsadm --list`

```bash
[root@wxd ~]# ipvsadm --list 
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
TCP  localhost:32642 rr
  -> 192.168.160.203:http         Masq    1      0          0         
  -> 192.168.160.204:http         Masq    1      0          0         

```