# DevOps-Project-7
![2 Tier App Deployment-k8s](https://github.com/rutikdevops/DevOps-Project-5/assets/109506158/d35ce1c1-785d-4619-928f-d42c9c9a1529)
<br></br>

# Project Blog link :-
<br></br>

# Project Overview :-
<br></br>

# Project Steps :-
- Create 2 ec2 instance : K8s-MASTER, K8s-WORKER  : Ubuntu , t2-Medium
- <img width="960" alt="image" src="https://github.com/rutikdevops/DevOps-Project-7/assets/109506158/aea1ee5d-e3d0-46e0-911d-b358f1f7016c">
- Goto Security-> security group-> Edit inbound rules-> Add rule-> choose All Traffic

# 1. Install and Configure the Docker :-
```bash
ubuntu
sudo su
apt update -y
apt install docker.io -y
hostnamectl set-hostname docker
bash
docker ps
whoami
chown $USER /var/run/docker.sock
```

# 2. Clone the Github code :-
```bash
git clone https://github.com/rutikdevops/DevOps-Project-5.git
cd DevOps-Project-5
ls
mkdir k8s1
ls
```
```bash
vi two-tier-app-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: two-tier-app-pod
spec:
  containers:
  - name: two-tier-app-pod
    image: rutikdevops/flaskapp
    env:
      - name: MYSQL_HOST
        value: "mysql"          # this is your mysql's service clusture IP, Make sure to change it with yours
      - name: MYSQL_PASSWORD
        value: "admin"
      - name: MYSQL_USER
        value: "root"
      - name: MYSQL_DB
        value: "mydb"
    ports:
      - containerPort: 5000
    imagePullPolicy: Always
```

```bash
kubectl apply -f two-tier-app-pod.yml
kubectl get pods
```

- Create Deployment pod
```bash
vi two-tier-app-deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: two-tier-app
  labels:
    app: two-tier-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: two-tier-app
  template:
    metadata:
      labels:
        app: two-tier-app
    spec:
      containers:
        - name: two-tier-app
          image: rutikdevops/flaskapp
          env:
            - name: MYSQL_HOST
              value: "mysql"          # this is your mysql's service clusture IP, Make sure to change it with yours
            - name: MYSQL_PASSWORD
              value: "admin"
            - name: MYSQL_USER
              value: "root"
            - name: MYSQL_DB
              value: "mydb"
          ports:
            - containerPort: 5000
          imagePullPolicy: Always
```

```bash
kubectl apply -f two-tier-app-deployment.yml
kubectl get pods
```


```bash
vi two-tier-app-svc.yml

apiVersion: v1
kind: Service
metadata:
  name: two-tier-app-service
spec:
  selector:
    app: two-tier-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
      nodePort: 30004
  type: NodePort
```

```bash
kubectl apply -f two-tier-app-svc.yml
kubectl get service
```


```bash
vi mysql-deployment.yml


```



















