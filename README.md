![Tetrisfinal](https://github.com/Narasimha76/Tetrisv1/assets/123463333/d45cad32-e516-4387-8eb4-dabc33a5d4ff)

Automating Tetris Deployments Using DevSecOps

 


GitHub REPOSITORIES :
TETRIS-VERSION1 : https://github.com/Narasimha76/Tetrisv1
TETRIS-VERSION2 : https://github.com/Narasimha76/TetrisV2
TETRIS_MANIFEST : https://github.com/Narasimha76/Tetris-manifest




Step1: Terraform Provisioning
•	Provision an AWS EC2 instance using Terraform, incorporating a bash script. This script                   orchestrates the installation of Docker, SonarQube, Trivy, OWASP, Terraform,  AWS CLI, Jenkins with JDK, and Kubectl on the EC2 instance.
 
 
 

 

•	Apply commands  terraform init and terrafom apply –auto-approve
•	Then the resource is creating in the aws.



Step2: Jenkins Pipeline for EKS
•	Create a pipeline to provision an EKS cluster and node group using Terraform.
•	Check all the scripts that are installed in the ec2 Server
 

•	Configure the Jenkins
 
•	Create a pipeline to provision an EKS cluster and node group using Terraform.
 
•	Eks  provision terraform code is present in git-hub
 
 
 

•	Eks cluster has been created in AWS  

 
 

Step3: Creating Full DevSecOps Pipeline
 1. Checkout the repository. 
 2. SonarQube analysis, including quality gate. 
 3. Install dependencies using npm. 
 4. OWASP Scan with Dependency Check. 
 5. Trivy Scan for file analysis. 
 6. Build Docker image and push to Docker Hub. 
 7. Trivy scan Docker image. 
 8. Trigger another pipeline (Manifest Pipeline). 

We need some plugins to complete this process
1. Eclipse Temurin installer
2. Sonarqube Scanner
3. NodeJs
4. Owasp Dependency-Check
5. Docker, Docker Commons, Docker Pipeline, Docker API, Docker-build-step

Add the tools in the Jenkins UI for JDK, Docker, NodeJS, Sonarqube, OWSAP(DependencyCheck)

•	Configure the Sonarqube by creating user token and that token to the Jenkins credentials and taken the credentials and create the system for the Jenkins in the Jenkins UI.

 
 


•	Now let’s create an pipeline 
 

•	Add the Jenkins Script in the pipeline which is present in my git repo

 

•	Now run the pipeline
 

•	Dependency-Check Results for the pipeline

 

•	Sonarqube check for the pipeline
 
Step4: Create a Manifest Pipeline 
•	Add this code to the Jenkins pipeline
 
Step5: Install ArgoCD on AWS EKS and configure it
•	Before installation add the eks cluster to your CLI
 
•	Install the ArgoCD
 
 
•	Now modify the argocd service to loadbalancer
 

 

•	Now copy the dns of the loadbalancer and paste in the web

 

•	Take the password from the secrets and decode it into bas64

 

 

 

Step6: Use ArgoCD to synchronize AWS EKS deployments with manifest changes in git repo
•	Create a project in the argocd with the connection of git-hub manifest repo and define the destination eks cluster in the aws eks.
  
•	It will monitors the manifest repo when ever the change occur in the repo it will keep the eks cluster in the desired state 
•	After deploying the pods and load balancer service in the eks cluster then we can access the game through the service endpoint

   

Step7: Install Prometheus, Grafana, and Node Exporter using Helm on AWS EKS.
•	First install the helm in the eks cluster.
•	Install the grafana and node exporter and grafana using helm commands in the eks cluster.
•	After installation edit the Prometheus file to add the targets of node exporter
 

 
•	Node Exporter metrics :
 

Grafana :
 


•	Login with grafana and add the the datasource as the Prometheus and create a dashboard with that Prometheus datasource
•	Then the dashboard of the eks cluster will be displayed
 

