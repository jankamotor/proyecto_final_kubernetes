# proyecto_final_kubernetes
Implementación de wordpress + mysql utilizando kubernetes

Instalación de Kubernetes con minikube

Hardware y OS utilizado:

• Debian 11 Server on Vmware Esxi
• 2 CPUs, 4GB free RAM, and 20GB free disk space
• User with sudo rights
• Virtualization Support enabled in the BIOS
• Working Internet connection

System update:

sudo apt update -y
sudo apt upgrade -y
sudo apt-get install apt-transport-https software-properties-common ca-certificates curl gnupg lsb-release -y

Install Docker:

curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli -y
docker version
sudo systemctl enable docker
sudo usermod -aG docker $USER
shutdown -r now

Download MiniKube:
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

Install and Start MiniKube on Debian 11:

sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version

Install Kubernetes command-line tool:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv kubectl /usr/local/bin/
kubectl version -o yaml

Deploy MiniKube on Debian 11:
minikube start
minikube status
minikube ssh
minikube addons list
kubectl cluster-info
kubectl get nodes

Application deployment:
- Log in to the server via ssh.
- Create a folder called projects.
- Create a folder called worpress inside projects.
- Open a text editor to create objects and services.

Frontend files:
- frontend-service.yml
- frontend-persistentvolumeclaim.yml
- frontend-deployment.yml

Backend files:
- backend-service.yml
- backend-persistentvolumeclaim.yml
- backend-deployment.yml

Secrets files:
- app-secret.yml
- app-configmap.yml

To create the objects run:
- kubectl create -f frontend-service.yml + ENTER
- kubectl create -f frontend-persistentvolumeclaim.yml + ENTER
- kubectl create -f frontend-deployment.yml + ENTER
- kubectl create -f backend-service.yml + ENTER
- kubectl create -f backend-persistentvolumeclaim.yml + ENTER
- kubectl create -f backend-deployment.yml + ENTER
- kubectl create -f app-secret.yml + ENTER
- kubectl create -f app-configmap.yml + ENTER

Verify the creation of pods:
- kubectl get pods

- administrator@minikube:~/proyects/wordpress$ kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
mysql-7c4d46dbd6-zlxrb       1/1     Running   0          7h10m
wordpress-7bc7d87c79-4hrff   1/1     Running   0          7h10m
wordpress-7bc7d87c79-g7hgt   1/1     Running   0          7h10m
wordpress-7bc7d87c79-zdbgs   1/1     Running   0          7h10m

Verification of services:
- kubectl get svc
- administrator@minikube:~/proyects/wordpress$ kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
frontend     NodePort    10.106.7.140     <none>        80:32351/TCP   7h13m
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        7h18m
mysql        ClusterIP   10.110.190.225   <none>        3306/TCP       7h12m
  
Verification of the Url of the application within the cluster:
- minikube service frontend --url
- administrator@minikube:~/proyects/wordpress$ minikube service frontend --url
http://192.168.49.2:32351
  
Proxy configuration for external access:
- kubectl port-forward --address 0.0.0.0 service/frontend 8080:80 

****************Enjoy the application*********************************



