

### nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
sepc:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort:80
  restartPolicy: OnFailure
```