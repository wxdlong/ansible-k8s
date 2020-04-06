## FAQ

1. Should Disable selinux

```bash
Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971096   13314 remote_runtime.go:105] RunPodSandbox from runtime service failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown
Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971146   13314 kuberuntime_sandbox.go:68] CreatePodSandbox for pod "kube-scheduler-wxd.long_kube-system(d4d13becd444ee4a984c5c68b2a69ffd)" failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown
Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971159   13314 kuberuntime_manager.go:729] createPodSandbox for pod "kube-scheduler-wxd.long_kube-system(d4d13becd444ee4a984c5c68b2a69ffd)" failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown
```


2. Missing double quote.  `error unmarshaling JSON: while decoding JSON: json: cannot unmarshal string`

```bash
[root@wxd ~]# kubeadm init --config kube_config_ipv6dual.yaml
W0329 07:52:12.290432   24499 strict.go:54] error unmarshaling configuration schema.GroupVersionKind{Group:"kubeadm.k8s.io", Version:"v1beta2", Kind:"ClusterConfiguration"}: error unmarshaling JSON: while decoding JSON: json: cannot unmarshal string into Go struct field ClusterConfiguration.featureGates of type map[string]bool
v1beta2.ClusterConfiguration.ControllerManager: v1beta2.ControlPlaneComponent.ExtraArgs: ReadString: expects " or n, but found 1, error found in #10 byte of ...|ze-ipv6":120,"servic|..., bigger context ...|:"IPv6DualStack=true","node-cidr-mask-size-ipv6":120,"service-cluster-ip-range":"fd03::/120"}},"dns"|...
To see the stack trace of this error execute with --v=5 or higher
```

3. IPV6 address can't be use because of use a scope link IPV6 address with prefix "fe80", change to another scope global IPV6.

```bash
cannot use "fe80::215:5dff:fe1f:f40b" as the bind address for the API Server
k8s.io/kubernetes/cmd/kubeadm/app/util/config.VerifyAPIServerBindAddress
```

4. Kube-proxy must use IPVS module, otherwise IPV6 can't work.


4. CoreDNS image is wrong with execute path didn't existed. Maybe pull image from office.


8. IPv6DualStack the InternalIP:  `172.17.55.115` always only IPv4 or IPV6

```bash
[root@wxd ~]# kubectl describe node ok8s
Name:               ok8s
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ok8s
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    projectcalico.org/IPv4Address: 172.17.55.115/28
                    projectcalico.org/IPv4IPIPTunnelAddr: 192.168.160.192
                    projectcalico.org/IPv6Address: fc00::24/64
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 30 Mar 2020 20:25:49 -0400
Taints:             node-role.kubernetes.io/master:NoSchedule
Unschedulable:      false
Lease:
  HolderIdentity:  ok8s
  AcquireTime:     <unset>
  RenewTime:       Mon, 30 Mar 2020 20:30:21 -0400
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Mon, 30 Mar 2020 20:29:45 -0400   Mon, 30 Mar 2020 20:29:45 -0400   CalicoIsUp                   Calico is running on this node
  MemoryPressure       False   Mon, 30 Mar 2020 20:29:52 -0400   Mon, 30 Mar 2020 20:25:49 -0400   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Mon, 30 Mar 2020 20:29:52 -0400   Mon, 30 Mar 2020 20:25:49 -0400   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Mon, 30 Mar 2020 20:29:52 -0400   Mon, 30 Mar 2020 20:25:49 -0400   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Mon, 30 Mar 2020 20:29:52 -0400   Mon, 30 Mar 2020 20:29:52 -0400   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.17.55.115
  Hostname:    ok8s

```

