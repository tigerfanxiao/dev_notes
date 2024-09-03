

查看 kubelet 的 log, 因为不是记录在 pod 上的. 所以只能从 node 上查看
```shell
sudo journalctl -u kubelet
```

查看 Kubernetes API server log
```shell
kubectl log -n kube-system <api_server_pod_name>
```

查看 container log
```shell
kubectl logs <pod_name> -c <coantianer_name>
```