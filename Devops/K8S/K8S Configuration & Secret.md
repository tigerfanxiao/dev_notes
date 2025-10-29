`my-configmap.yml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata: 
  name: my-configmap
data:
  key1: Hello World!
  key2: |
    Test
    Multiple Lines 
```

创建 configmap 对象
```shell
kubectl create -f my-configmap.yml
```

在文件中保存的密码需要先用 base 加密
```shell
echo -n "mysecret" | base64
```
make secrete file `mysecret.yml`
```yaml
apiVersion: v1
kind: Secret
metadata: 
  name: my-secret
type: Opaque
data:
  secretkey1: dddd
  secretkey2: bbbb
```
create secret
```shell
kubectl create -f my-secret.yml
```

### 使用命令创建 secret

```shell
kubectl create secret generic nginx-htpasswd --from-file .htpasswd
```


pass configuration data to our pods

```shell
apiVersion: v1
kind: Pod
metadata:
  name: env-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo "configmap: $CONFIGMAPVAR secret: $SECRETVAR"']
    env:
    - name: CONFIGMAPVAR
	  valueFrom:
	    configMapKeyRef:
	      name: my-configmap
	      key: key1
	- name: SECRETVAR
	  valueFrom:
	    secretKeyRef:
	      name: my-secret
	      key: secretkey1

```

create pod
```shell
kubectl create -f env-pod.yml

# 查看 logs
kubectl logs env-pod

```

# Configuration Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'while true; do sleep 3600; done']
    volumeMounts:
    - name: configmap-volume # 和下面真实 volume 的名字保持一致
      mountPath: /etc/config/configmap # 这是容器中的路径
    - name: secret-volume
      mountPath: /etc/config/secret
  volumes:
  - name: configmap-volume # 这里是已经创建的 configmap 对象
    configMap:
      name: my-configmap
  - name: secret-volume
    secret: 
      secretName: my-secret # 这里是已经创建的 secret 对象
```

创建 pod
```shell
kubectl create -f volume-pod.yml
# 向 pod 中所有容器下发命令
kubectl exec volume-pod -- ls /etc/config/configmap
# 此时可以看到 config 中的键值对生成的文件. 文件名是key的名字, 内容是 value

```


```shell

# 查看所有 configmap
kubectl get cm 
NAME               DATA   AGE
kube-root-ca.crt   1      40m
nginx-config       1      39m
# 查看具体的 configmap
kubectl describe cm nginx-config 

```