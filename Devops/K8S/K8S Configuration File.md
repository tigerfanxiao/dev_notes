K8S Configuration file 一般有 3 个部分组成
- metadata
- specification
- status
# Service

```yaml
apiVersion: app/V1
kind: Deployment
metadata:
	name: nginx-deployment
	labels: 
spec:
	replicas: 2
	selector:
	template: 
```

# Deployment

check desired state and actual state

```yaml
apiVersion: app/V1
kind: Service
metadata:
	name: nginx-service
spec:
	selector: 
	ports:
```