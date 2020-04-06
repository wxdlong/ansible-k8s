
## What

Offline install k8s signle node ENV.

## Docker

https://download.docker.com/linux/static/stable/x86_64/


## ENV

1. Letting iptables see bridged traffic
```bash
cat <<EOF > /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-arptables = 1
EOF
sysctl -p /etc/sysctl.d/k8s.conf
```

`sysctl -n net.bridge.bridge-nf-call-ip6tables` watch currently value, if error, then linux module `br_netfilter`didn't installed.
 
modprobe br_netfilter 

2. Disable firewalld

```bash
iptables -F && iptables -X \
        && iptables -F -t nat && iptables -X -t nat \
        && iptables -F -t raw && iptables -X -t raw \
        && iptables -F -t mangle && iptables -X -t mangle
        
    systemctl disable firewalld
```

3. Disable Selinx.
```bash
setenforce 0  && echo "SELINUX=disabled" > /etc/selinux/config
```

4. Disable swap. `swapoff -a && sysctl -w vm.swappiness=0`  and comment the swap in `fstab`.

```bash
 [root@wxd ~]# cat /etc/fstab 
    #
    # /etc/fstab
    # Created by anaconda on Fri Nov 29 22:05:35 2019
    #
    # Accessible filesystems, by reference, are maintained under '/dev/disk'
    # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
    #
    /dev/mapper/centos-root /                       xfs     defaults        0 0
    UUID=d5aaeea4-2629-4b48-b7d8-5853598db629 /boot                   xfs     defaults        0 0
    /dev/mapper/centos-home /home                   xfs     defaults        0 0
    ##/dev/mapper/centos-swap swap                    swap    defaults        0 0
    ```
```

5. Install kubeadm. 

```bash
CNI_VERSION="v0.8.2"
mkdir -p /opt/cni/bin
curl -L "https://github.com/containernetworking/plugins/releases/download/${CNI_VERSION}/cni-plugins-linux-amd64-${CNI_VERSION}.tgz" | tar -C /opt/cni/bin -xz
```

```bash
CRICTL_VERSION="v1.16.0"
mkdir -p /opt/bin
curl -L "https://github.com/kubernetes-sigs/cri-tools/releases/download/${CRICTL_VERSION}/crictl-${CRICTL_VERSION}-linux-amd64.tar.gz" | tar -C /opt/bin -xz
```


```bash
RELEASE="v1.17.4"

mkdir -p /opt/bin
cd /opt/bin
curl -L --remote-name-all https://storage.googleapis.com/kubernetes-release/release/${RELEASE}/bin/linux/amd64/{kubeadm,kubelet,kubectl}
chmod +x {kubeadm,kubelet,kubectl}

curl -sSL "https://raw.githubusercontent.com/kubernetes/kubernetes/${RELEASE}/build/debs/kubelet.service" | sed "s:/usr/bin:/opt/bin:g" > /etc/systemd/system/kubelet.service
mkdir -p /etc/systemd/system/kubelet.service.d
curl -sSL "https://raw.githubusercontent.com/kubernetes/kubernetes/${RELEASE}/build/debs/10-kubeadm.conf" | sed "s:/usr/bin:/opt/bin:g" > /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```



kubeadm init --pod-network-cidr fd04::/122 --service-cidr fd03::/122 --apiserver-advertise-address  fe80::215:5dff:fe1f:f40b --control-plane-endpoint fe80::215:5dff:fe1f:f40b --kubernetes-version v1.17.4 --v=5

docker pull registry.cn-hangzhou.aliyuncs.com/wxdlong/ok8s:v1.17.4


6. Install [Calico](https://docs.projectcalico.org/)

7. Make k8s master node as worker node. `kubectl taint nodes --all node-role.kubernetes.io/master-`

8. Deployer nginx service.



 