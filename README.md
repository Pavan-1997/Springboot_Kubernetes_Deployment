# Springboot application deployed on  Minikube Kubernetes cluster integrating MySQL DB to load data and view it using PostMan API calls, Finally visualizing the Kubenetes Dashboard.

Below are the steps documented for the same:

1. Launch an instance from an Amazon Linux 2 or Amazon Linux AMI with t2.medium


2. Connect to your instance


3. Update the packages and package caches you have installed on your instance:

yum update -y


4. Install the latest Docker Engine packages:

amazon-linux-extras install docker 
OR
yum install docker -y

docker -v


5. Start and enable the Docker as a service:

systemctl start docker 

systemctl enable docker


6. Install Conntrack(software for minikube-kubernetes to run) and Git:

yum install conntrack -y

yum install git -y


7. Install Minikube-Kubernetes:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube


8. Start Minikube:

/usr/local/bin/minikube start --force --driver=docker

/usr/local/bin/minikube version


9. Install kubectl:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

/usr/local/bin/kubectl version


9.1 Clone the Repo:

cd /opt/

git clone https://github.com/DEVOPS-WITH-WEB-DEV/springboot-k8s.git


10. Make the DB Up and Setup the DB:

/usr/local/bin/kubectl create -f db-deployment.yaml

/usr/local/bin/kubectl get pods

/usr/local/bin/kubectl exec -it <POD-NAME> /bin/bash


10.1 Login to the DB:

mysql -u root -p 

Password: root


10.2 Show DBs:

show databases;


11. Install Maven:

yum install maven -y	

mvn -version


12. Create the docker image: 

docker build -t pavanraj97/springboot-crud-k8s:1.0 .


13. Do a docker login:

docker login

Give dockerhub username and password

13.1 Push the docker image

docker push pavanraj97/springboot-crud-k8s:1.0


14. Make the App Up:

/usr/local/bin/kubectl  apply -f app-deployment.yaml


15. Check the services:

/usr/local/bin/kubectl  get svc


16. Check the IP of the Minikube:

/usr/local/bin/minikube ip

17. Apply Port Forwarding:

/usr/local/bin/kubectl port-forward --address 0.0.0.0 svc/springboot-crud-svc 8080:8080 &


18. Post a JSON data using Postman:

http://<EC2IP>:8080/orders

Send the below data in Postman using RAW

{
    "name": "Raj",
    "qty": 5,
    "price": 150.0
}


18.1 Check the DB in EC2:

Follow Step 10 to login POD and then DB 


18.2 Change the DB:

use singamlabs;


18.3 Check the tables in the above DB:

show tables;


18.4 Check the rows in the table:

select * from orders_tbl;


19. For Kubernetes Dashboard:

In First Terminal:

/usr/local/bin/kubectl proxy --address='0.0.0.0' --accept-hosts='^*$'

In Secod Terminal:

/usr/local/bin/minikube dashboard


20. Hit the below URL to view the dashboard:

http://<EC2IP>:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/pod?namespace=default
