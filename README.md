项目多应用，多环境的例子

目录结构
```
[root@hjfeng example]# tree
.
├── app1
│   ├── base
│   │   └── kustomization.yaml
│   ├── dev
│   │   ├── kustomization.yaml
│   │   └── patch.yaml
│   ├── prod
│   │   ├── kustomization.yaml
│   │   └── patch.yaml
│   └── staging
│       ├── kustomization.yaml
│       └── patch.yaml
├── app2
├── base
│   ├── deployment.yaml
│   └── kustomization.yaml
└── README.md

7 directories, 10 files

```

项目级模板
```
[root@hjfeng example]# kustomize build base
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: app
    k8s-project: project
  name: app
spec:
  selector:
    matchLabels:
      k8s-app: app
      k8s-project: project
  template:
    metadata:
      labels:
        k8s-app: app
        k8s-project: project
    spec:
      containers:
      - image: app:v0.0.0
        name: app
```

应用级模板
```
[root@hjfeng example]# kustomize build app1/base
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: app1
    k8s-project: project1
  name: app1
spec:
  selector:
    matchLabels:
      k8s-app: app1
      k8s-project: project1
  template:
    metadata:
      labels:
        k8s-app: app1
        k8s-project: project1
    spec:
      containers:
      - image: app1:v0.0.0
        name: app1
```

应用app1 dev环境
```
[root@hjfeng example]# kustomize build app1/dev
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: app1
    k8s-project: project1
  name: app1
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: app1
      k8s-project: project1
  template:
    metadata:
      labels:
        k8s-app: app1
        k8s-project: project1
    spec:
      containers:
      - image: nginx:1.20.0
        name: app1
```

应用app1 staging环境
```
[root@hjfeng example]# kustomize build app1/staging
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: app1
    k8s-project: project1
  name: app1
spec:
  replicas: 2
  selector:
    matchLabels:
      k8s-app: app1
      k8s-project: project1
  template:
    metadata:
      labels:
        k8s-app: app1
        k8s-project: project1
    spec:
      containers:
      - image: nginx:1.20.1
        name: app1
```

应用app1 prod环境
```
[root@hjfeng example]# kustomize build app1/prod
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: app1
    k8s-project: project1
  name: app1
spec:
  replicas: 3
  selector:
    matchLabels:
      k8s-app: app1
      k8s-project: project1
  template:
    metadata:
      labels:
        k8s-app: app1
        k8s-project: project1
    spec:
      containers:
      - image: nginx:1.20.2
        name: app1
```
