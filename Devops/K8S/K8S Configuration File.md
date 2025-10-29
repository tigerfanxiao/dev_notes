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
kind: Deployment
metadata:
	name: my-deployment
	labels:
	  app: my-deployment
spec:
	replicas: 2
	selector: 
	  matchLabels:
	    app: my-deployment
	temlate:
	  metadata:
	    labels:
	      app: my-deployment
	  spec:
	    containers:
		- name: nginx
		  image: nginx:1.14.2
		  ports:
		  - containerPort: 80
```

