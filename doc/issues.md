

1. 

Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971096   13314 remote_runtime.go:105] RunPodSandbox from runtime service failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown
Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971146   13314 kuberuntime_sandbox.go:68] CreatePodSandbox for pod "kube-scheduler-wxd.long_kube-system(d4d13becd444ee4a984c5c68b2a69ffd)" failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown
Mar 28 08:32:36 wxd kubelet: E0328 08:32:36.971159   13314 kuberuntime_manager.go:729] createPodSandbox for pod "kube-scheduler-wxd.long_kube-system(d4d13becd444ee4a984c5c68b2a69ffd)" failed: rpc error: code = Unknown desc = failed to start sandbox container for pod "kube-scheduler-wxd.long": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"write /proc/self/attr/keycreate: permission denied\"": unknown




2. 配置出错

   string - String yaml配置的时候需要加双引号
[root@wxd ~]# kubeadm init --config kube_config_ipv6dual.yaml
W0329 07:52:12.290432   24499 strict.go:54] error unmarshaling configuration schema.GroupVersionKind{Group:"kubeadm.k8s.io", Version:"v1beta2", Kind:"ClusterConfiguration"}: error unmarshaling JSON: while decoding JSON: json: cannot unmarshal string into Go struct field ClusterConfiguration.featureGates of type map[string]bool
v1beta2.ClusterConfiguration.ControllerManager: v1beta2.ControlPlaneComponent.ExtraArgs: ReadString: expects " or n, but found 1, error found in #10 byte of ...|ze-ipv6":120,"servic|..., bigger context ...|:"IPv6DualStack=true","node-cidr-mask-size-ipv6":120,"service-cluster-ip-range":"fd03::/120"}},"dns"|...
To see the stack trace of this error execute with --v=5 or higher
