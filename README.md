1️⃣ Create Your Spring Boot App
		a)mvn clean package

		b)create Dockerfile
2️⃣ Create Docker Image
	
		docker build -t springboot-myeks2 .
3️⃣ Create ECR Repository(as your region)

		aws ecr create-repository --repository-name springboot-myeks2 --region us-east-2.

This gives you a URI like:165220828221.dkr.ecr.us-east-2.amazonaws.com/springboot-myeks2

4️⃣ Login to ECR

		aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 165220828221.dkr.ecr.us-east-2.amazonaws.com

5️⃣ Tag & Push Docker Image to ECR

		docker tag springboot-myeks2:latest 165220828221.dkr.ecr.us-east-2.amazonaws.com/springboot-myeks2:latest

		docker push 165220828221.dkr.ecr.us-east-2.amazonaws.com/springboot-myeks2:latest
6️⃣ Create EKS Cluster
		
		eksctl create cluster --name jt-cluster --version 1.28 --nodes=1 --node-type t2.small --region us-east-2 --with-oidc

7️⃣ Update Kubeconfig
		
 		aws eks --region us-east-2 update-kubeconfig --name jt-cluster

		kubectl get nodes
8️⃣ Deploy Your App on EKS
Create Kubernetes Deployment YAML 
Create Service YAML 

		kubectl apply -f springboot-deployment.yaml
		kubectl apply -f springboot-service.yaml

9️⃣ Verify
		kubectl get pods
		kubectl get svc springboot-service

u will get EXTERNAL-IP. Open the EXTERNAL-IP in your browser to access your app!


