THE ULTIMATE CICD
DEVOPS PIPELINE PROJECT

# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- HTML5
- CSS
- Maven
- Docker
- Jenkins
- Kubernetes
- SonarQube
- Nexus
- prometheus
- Grafana

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

PHASE-1 | Setup Infra

To create an Ubuntu EC2 instance in AWS, follow these steps:
1. Sign in to the AWS Management Console:
o Go to the AWS Management Console
at https://aws.amazon.com/console/.
o Sign in with your AWS account credentials.
2. Navigate to EC2:
o Once logged in, navigate to the EC2 dashboard by typing "EC2" in the
search bar at the top or by selecting "Services" and then "EC2" under
the "Compute" section.
3. Launch Instance:
o Click on the "Instances" link in the EC2 dashboard sidebar.
o Click the "Launch Instance" button.
4. Choose an Amazon Machine Image (AMI):
o In the "Step 1: Choose an Amazon Machine Image (AMI)" section, select
"Ubuntu" from the list of available AMIs.
o Choose the Ubuntu version you want to use. For example, "Ubuntu
Server 24.04 LTS".
o Click "Select".
5. Choose an Instance Type:
o In the "Step 2: Choose an Instance Type" section, select the instance
type that fits your requirements. The default option (usually a t2.micro
instance) is suitable for testing and small workloads.
o Click "Next: Configure Instance Details".
6. Configure Instance Details:
o Optionally, configure instance details such as network settings, subnets,
IAM role, etc. You can leave these settings as default for now.
o Click "Next: Add Storage".
7. Add Storage:
o Specify the size of the root volume (default is usually fine for testing
purposes).
o Click "Next: Add Tags".
8. Add Tags:
o Optionally, add tags to your instance for better organization and
management.
o Click "Next: Configure Security Group".
9. Configure Security Group:
o In the "Step 6: Configure Security Group" section, configure the security
group to allow SSH access (port 22) from your IP address.
o You may also want to allow other
o Click "Review and Launch".
10. Review and Launch:
o Review the configuration of your instance.
o Click "Launch".
11. Select Key Pair:
o In the pop-up window, select an existing key pair or create a new one.
o Check the acknowledgment box.
o Click "Launch Instances".
12. Access Your Instance:
o Use Mobaxterm


Setup K8-Cluster using kubeadm
[K8 Version-->1.28.1]
1. Update System Packages [On Master & Worker Node]
sudo apt-get update
2. Install Docker[On Master & Worker Node]
sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock
3. Install Required Dependencies for Kubernetes[On Master & Worker
Node]
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p -m 755 /etc/apt/keyrings
4. Add Kubernetes Repository and GPG Key[On Master & Worker Node]
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --
dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg]
https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee
/etc/apt/sources.list.d/kubernetes.list
5. Update Package List[On Master & Worker Node]
sudo apt update
6. Install Kubernetes Components[On Master & Worker Node]
sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1
kubeadm - Going to setup a Kubernetes cluster
kubelet - Responsible for creating pods which we are going to deploy applications
kubectl - will work as cli to interact with the k8s cluster
7. Initialize Kubernetes Master Node [On MasterNode]
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
After run the above command then our vm will acts as master node and it will generate token to
connect this with slave node -copy the token and run the command in slave machines 1 & 2
8. Configure Kubernetes Cluster [On MasterNode]
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
9. Deploy Networking Solution (Calico) [On MasterNode]
kubectl apply -f https://docs.projectcalico.org/v3.20/manifests/calico.yaml
10. Deploy Ingress Controller (NGINX) [On MasterNode]
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingressnginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml
11. We'll Scan Kubernetes Cluster For Any Kind Of Issues For That We Have Multiple Tools Like
Trivy Also We Have But Trivy May Not Work Always So For That Reason We Are Just Going To Go
With Cube Audit It Also A Tool That Can Be Used For Scanning
->https://github.com/shopify/kubeaudit/releases (Go To The Web Site Copy The Linux_amd_64
Link)
->Paste It Using wget Command
->Now Untar The File Using tar -xvf File Name
->sudo mv kubeaudit /usr/local/bin/
->kubeaudit all

Installing Jenkins on Ubuntu
#!/bin/bash
# Install OpenJDK 17 JRE Headless
sudo apt install openjdk-17-jre-headless -y
# Download Jenkins GPG key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
# Add Jenkins repository to package manager sources
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
# Update package manager repositories
sudo apt-get update
# Install Jenkins
sudo apt-get install jenkins -y
Save this script in a file, for example, install_jenkins.sh, and make it executable
using:
chmod +x install_jenkins.sh
Then, you can run the script using:
./install_jenkins.sh
This script will automate the installation process of OpenJDK 17 JRE Headless and
Jenkins.
Now we can able to access Jenkins :using the public ip address http://IP:8080
Use the below command on your jenkins machine
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Get password it will be like this —> a771e06575b54f91bc56a42ccdbb2f76
Install the suggested packages and create user with your name and password



Install docker and trivy on Jenkins machine for future use
#!/bin/bash
# Update package manager repositories
sudo apt-get update
# Install necessary dependencies
sudo apt-get install -y ca-certificates curl
# Create directory for Docker GPG key
sudo install -m 0755 -d
/etc/apt/keyrings
# Download Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o
/etc/apt/keyrings/docker.asc
# Ensure proper permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add Docker repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signedby=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Update package manager repositories
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin
docker-compose-plugin
Save this script in a file, for example, install_docker.sh, and make it executable
using:
chmod +x install_docker.sh
Then, you can run the script using:
./install_docker.sh
Trivy Installation Steps:
#!/bin/bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee
/usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg]
https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a
/etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
After creating the script change the permission to executable file
chmod +x trivy.sh
ls
./trivy.sh
trivy –version


SetUp Nexus
#!/bin/bash
# Update package manager repositories
sudo apt-get update
# Install necessary dependencies
sudo apt-get install -y ca-certificates curl
# Create directory for Docker GPG key
sudo install -m 0755 -d
/etc/apt/keyrings
# Download Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o
/etc/apt/keyrings/docker.asc
# Ensure proper permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add Docker repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signedby=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Update package manager repositories
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin
docker-compose-plugin
Save this script in a file, for example, install_docker.sh, and make it executable
using:
chmod +x install_docker.sh
Then, you can run the script using:
./install_docker.sh
Create Nexus using docker container
To create a Docker container running Nexus 3 and exposing it on port 8081, you can
use the following command:
docker run -d --name nexus -p 8081:8081 sonatype/nexus3:latest
This command does the following:
● -d: Detaches the container and runs it in the background.
● --name nexus: Specifies the name of the container as "nexus".
● -p 8081:8081: Maps port 8081 on the host to port 8081 on the container, allowing
access to Nexus through port 8081.
● sonatype/nexus3:latest: Specifies the Docker image to use for the container, in this
case, the latest version of Nexus 3 from the Sonatype repository.
After running this command, Nexus will be accessible on your host machine
at http://IP:8081.

Get Nexus initial password
Your provided commands are correct for accessing the Nexus password stored in the
container. Here's a breakdown of the steps:
1. Get Container ID: You need to find out the ID of the Nexus container. You
can do this by running:
docker ps
This command lists all running containers along with their IDs, among other
information.
2. Access Container's Bash Shell: Once you have the container ID, you can
execute the docker exec command to access the container's bash shell:
docker exec -it <container_ID> /bin/bash
Replace <container_ID> with the actual ID of the Nexus container.
3. Navigate to Nexus Directory: Inside the container's bash shell, navigate to the
directory where Nexus stores its configuration:
cd sonatype-work/nexus3
4. View Admin Password: Finally, you can view the admin password by
displaying the contents of the admin.password file:
cat admin.password
5. Exit the Container Shell: Once you have retrieved the password, you can exit
the container's bash shell:
exit
This process allows you to access the Nexus admin password stored within the
container. Make sure to keep this password secure, as it grants administrative access
to your Nexus instance


SetUp SonarQube
#!/bin/bash
# Update package manager repositories
sudo apt-get update
# Install necessary dependencies
sudo apt-get install -y ca-certificates curl
# Create directory for Docker GPG key
sudo install -m 0755 -d
/etc/apt/keyrings
# Download Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o
/etc/apt/keyrings/docker.asc
# Ensure proper permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add Docker repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signedby=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Update package manager repositories
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin
docker-compose-plugin
Save this script in a file, for example, install_docker.sh, and make it executable
using:
chmod +x install_docker.sh
Then, you can run the script using:
./install_docker.sh
Create Sonarqube Docker container
To run SonarQube in a Docker container with the provided command, you can follow
these steps:
1. Open your terminal or command prompt.
2. Run the following command:
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
This command will download the sonarqube:lts-community Docker image from Docker
Hub if it's not already available locally. Then, it will create a container named "sonar
from this image, running it in detached mode (-d flag) and mapping port 9000 on the
host machine to port 9000 in the container (-p 9000:9000 flag).
3. Access SonarQube by opening a web browser and navigating to http://VmIP:9000.
This will start the SonarQube server, and you should be able to access it using the
provided URL. If you're running Docker on a remote server or a different port, replace
localhost with the appropriate hostname or IP address and adjust the port
accordingly



PHASE-2 | Private Git Setup
Steps to create a private Git repository, generate a personal access token,
connect to the repository, and push code to it:
1. Create a Private Git Repository:
o Go to your preferred Git hosting platform (e.g., GitHub, GitLab, Bitbucket).
o Log in to your account or sign up if you don't have one.
o Create a new repository and set it as private.
2. Generate a Personal Access Token:
o Navigate to your account settings or profile settings.
o Look for the "Developer settings" or "Personal access tokens" section.
o Generate a new token, providing it with the necessary permissions (e.g., repo
access).
3. Clone the Repository Locally:
o Open Git Bash or your terminal.
o Navigate to the directory where you want to clone the repository.
o Use the git clone command followed by the repository's URL. For example:
git clone <repository_URL>
4. Replace <repository_URL> with the URL of your private repository.
5. Add Your Source Code Files:
o Navigate into the cloned repository directory.
o Paste your source code files or create new ones inside this directory.
6. Stage and Commit Changes:
o Use the git add command to stage the changes:
git add .
o Use the git commit command to commit the staged changes along with a
meaningful message:
git commit -m "Your commit message here"
7. Push Changes to the Repository:
o Use the git push command to push your committed changes to the remote
repository
git push
o If it's your first time pushing to this repository, you might need to specify the
remote and branch:
git push -u origin master
8. Replace master with the branch name if you're pushing to a different branch.
9. Enter Personal Access Token as Authentication:
o When prompted for credentials during the push, enter your username (usually
your email) and use your personal access token as the password.
By following these steps, you'll be able to create a private Git repository, connect to it
using Git Bash, and push your code changes securely using a personal access token for
authentication.


PHASE-3 | CICD
Install below Plugins in Jenkins
1. Eclipse Temurin Installer:
o This plugin enables Jenkins to automatically install and configure the Eclipse
Temurin JDK (formerly known as AdoptOpenJDK).
o To install, go to Jenkins dashboard -> Manage Jenkins -> Manage Plugins ->
Available tab.
o Search for "Eclipse Temurin Installer" and select it.
o Click on the "Install without restart" button.
2. Pipeline Maven Integration:
o This plugin provides Maven support for Jenkins Pipeline.
o It allows you to use Maven commands directly within your Jenkins Pipeline
scripts.
o To install, follow the same steps as above, but search for "Pipeline Maven
Integration" instead.
3. Config File Provider:
o This plugin allows you to define configuration files (e.g., properties, XML,
JSON) centrally in Jenkins.
o These configurations can then be referenced and used by your Jenkins jobs.
o Install it using the same procedure as mentioned earlier
4. SonarQube Scanner:
o SonarQube is a code quality and security analysis tool.
o This plugin integrates Jenkins with SonarQube by providing a scanner that
analyzes code during builds.
o You can install it from the Jenkins plugin manager as described above.
5. Kubernetes CLI:
o This plugin allows Jenkins to interact with Kubernetes clusters using the
Kubernetes command-line tool (kubectl).
o It's useful for tasks like deploying applications to Kubernetes from Jenkins
jobs.
o Install it through the plugin manager.
6. Kubernetes:
o This plugin integrates Jenkins with Kubernetes by allowing Jenkins agents to
run as pods within a Kubernetes cluster.
o It provides dynamic scaling and resource optimization capabilities for Jenkins
builds.
o Install it from the Jenkins plugin manager.
7. Docker:
o This plugin allows Jenkins to interact with Docker, enabling Docker builds and
integration with Docker registries.
o You can use it to build Docker images, run Docker containers, and push/pull
images from Docker registries.
o Install it from the plugin manager.
8. Docker Pipeline Step:
o This plugin extends Jenkins Pipeline with steps to build, publish, and run
Docker containers as part of your Pipeline scripts.
o It provides a convenient way to manage Docker containers directly from
Jenkins Pipelines.
o Install it through the plugin manager like the others.
After installing these plugins, you may need to configure them according to your
specific environment and requirements. This typically involves setting up credentials,
configuring paths, and specifying options in Jenkins global configuration or individual
job configurations. Each plugin usually comes with its own set of documentation to
guide you through the configuration process.
Configure Above Plugins in Jenkins Pipeline
Configure the tools choose manage jenkins → Tools
Choose jdk and fill as :
name= jdk17
check on install automatically and choose install from adoptium.net

choose sonarqube scanner and configure
name=sonar-scanner
check on install automatically from Maven Central
Version= sonarqube scanner 6.1.0.4477

choose maven and Configure
name= maven3
check on install automatically from apache
version=3.6.1

choose Docker and Configure
name= docker
check on install automatically from docker.com
version=latest

Create the new credentials for sonarqube
Get the token from sonarcube administration→ security—>generate token
(squ_db0be0fe8e1896ad726617530d37cf233a82993d)

now open jenkins and go on this path dashboard- manage jenkins - credentials- system- global credentials.
kind= secret text
scope= global jenkins
secret= paste the key
id= sonar scanner


Now need to configure in the sonarqube server
Go to manage jenkins >system
Sonar Qube installation

Name= sonar
server url= paste the sonar qube url
server authentication= sonar-scanner

Now go to the Dashboard, click on the pipeline and paste the below script

pipeline {
agent any
tools {
jdk 'jdk17'
maven 'maven3'
}
environment {
SCANNER_HOME= tool 'sonar-scanner'
}
stages {
stage('Git Checkout') {
steps {
git branch: 'main', credentialsId: 'git-cred', url:
'https://github.com/kumar-sachin0/CICD-Project
'
}
}
stage('Compile') {
steps {
sh "mvn compile"
}
}
stage('Test') {
steps {
sh "mvn test"
}
}
stage('File System Scan') {
steps {
sh "trivy fs --format table -o trivy-fs-report.html ."
}
}
stage('SonarQube Analsyis') {
steps {
withSonarQubeEnv('sonar') {
sh ''' $SCANNER_HOME/bin/sonar-scanner -
Dsonar.projectName=BoardGame -Dsonar.projectKey=BoardGame \
-Dsonar.java.binaries=. '''
}
}
}
stage('Quality Gate') {
steps {
script {
waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
}
}
}
stage('Build') {
steps {
sh "mvn package"
}
}
stage('Publish To Nexus') {
steps {
withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17',
maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
sh "mvn deploy"
}
}
}
stage('Build & Tag Docker Image') {
steps {
script {
withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
sh "docker build -t kumar-sachin0/boardshack:latest ."
}
}
}
}
stage('Docker Image Scan') {
steps {
sh "trivy image --format table -o trivy-image-report.html
kumar-sachin0/boardshack:latest "
}
}
stage('Push Docker Image') {
steps {
script {
withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
sh "docker push kumar-sachin0/boardshack:latest"
}
}
}
}
stage('Deploy To Kubernetes') {
steps {
withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '',
credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false,
serverUrl: 'https://172.31.8.146:6443') {
sh "kubectl apply -f deployment-service.yaml"
}
}
}
stage('Verify the Deployment') {
steps {
withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '',
credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false,
serverUrl: 'https://172.31.8.146:6443') {
sh "kubectl get pods -n webapps"
sh "kubectl get svc -n webapps"
}
}
}
}
post {
always {
script {
def jobName = env.JOB_NAME
def buildNumber = env.BUILD_NUMBER
def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'
def body = """
<html>
<body>
<div style="border: 4px solid ${bannerColor}; padding:
10px;">
10px;">
<h2>${jobName} - Build
${buildNumber}</h2>
<div style="background-color:
${bannerColor}; padding:
<h3 style="color: white;">Pipeline
Status:
${pipelineStatus.toUpperCase()}</h3>
</div>
<p>Check the <a href="${BUILD_URL}">console
output</a>.</p>
"""
</div>
</body>
</html>
emailext (
subject: "${jobName} - Build ${buildNumber} -
${pipelineStatus.toUpperCase()}",
body: body,
to:
'ganeshperumal882000@gmail.co
m', from: 'jenkins@example.com',
replyTo: 'jenkins@example.com',
mimeType: 'text/html',
attachmentsPattern: 'trivy-image-report.html'
)
}
}
}
}



Now we can able to access the see our application is running in slave machine 1&2
Check the ip address of both slave 1 & 2 http://yourip:31508/

If You face issue while rebuilding the artifacts is not uploaded , Allow
re-deploy in the settings
choose settings —>administration —> repositories —> maven-releases
there you can enable the re-deploy option


PHASE-4 | Monitoring
Launch an EC2 instances with the t2.medium and start setup installation prometheus
and Grafana

Before Starting Installation Update the package
sudo apt update
1.Links to download Prometheus, Node_Exporter & black Box
exporter https://prometheus.io/download/
Download the latest prometheus
Copy the link from the above official document website and download in your
local machine
wget
https://github.com/prometheus/prometheus/releases/download/v2.54.0/prometheu
s-2.54.0.linux-amd64.tar.gz
Extract the downloaded archive
tar -xvf prometheus-2.54.0.linux-amd64.tar.gz
cd prometheus-2.54.0.linux-amd64/
Now give ls we can able to see the executable file in the name of prometheus
,To start prometheus run the executable script in the name prometheus
./prometheus.sh &
Now we can able to access the prometheus ,http://<your_server_IP>:9090/.

2.Links to download
Grafana https://grafana.com/grafana/download
Refer the official documentation and install the latest version
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_11.1.4_amd64.deb
sudo dpkg -i grafana-enterprise_11.1.4_amd64.deb
### You can start grafana-server by executing
sudo /bin/systemctl start grafana-server
Grafana is running now, and we can connect to it at http://your.server.ip:3000 The
default user & password is admin / admin.


3.https://github.com/prometheus/blackbox_exporter
Download blackbox exporter from the official website of prometheus,
wget
https://github.com/prometheus/blackbox_exporter/releases/download/v0.25.0/blackbox_exporter-0.25.0.linux-amd64.tar.gz
Now Extract the tar file of blackbox exporter
tar -xvf blackbox_exporter-0.25.0.linux-amd64.tar.gz
To start the black box exporter

cd blackbox_exporter-0.25.0.linux-amd64/
Now give ls we can able to see the executable file in the name of blackbox ,To start blackbox
run the executable script in the name blackbox
./blackbox_exporter &
Now we can able to access the prometheus ,http://<your_server_IP>:9115/.


Prometheus yaml Configuration
cd prometheus-2.54.0.linux-amd64/
ls
vi prometheus.yml
# Load rules once and periodically evaluate them according to the global
'evaluation_interval'.
rule_files:
# - "first_rules.yml"
# - "second_rules.yml"
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
# The job name is added as a label `job=<job_name>` to any timeseries scraped from this
config.
- job_name: "prometheus"
# metrics_path defaults to '/metrics'
# scheme defaults to 'http'.
static_configs:
- targets: ["localhost:9090"]
- job_name: 'blackbox'
metrics_path: /probe
params:
module: [http_2xx] # Look for a HTTP 200 response.
static_configs:
- targets:
- http://prometheus.io # Target to probe with http
- http://13.201.188.1:31508/ # Target to probe with http on port 31508 our application.relabel_configs:
- source_labels: [__address__]
target_label: __param_target
- source_labels: [__param_target]
target_label: instance
- target_label: __address__
replacement: 3.110.151.216:9115 # The blackbox exporter's real hostname:127.0.0.1:


Now need to restart the prometheus
pgrep prometheus
it will shows the id
kill the id
kill 1992
Then restart
./prometheus &

NOW We Need to add prometheus as our datasource in Grafana
Then Import dashboard give ID 7587 to create black box exporter
dashboard

let us monitor the jenkins machine metrics by node exporter
install prometheus metrics plugins in jenkins

let us download the node exporter in jenkins machine
wget
https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_expor
ter-1.8.2.linux-amd64.tar.gz
Now extract the file
tar -xvf node_exporter-1.8.2.linux-amd64.tar.gz
Now run the nodeexporter script
cd node_exporter-1.8.2.linux-amd64/
ls
./node_exporter &
Now nodeExporter can be accessible through the browser
http://<your_server_IP>:9100

now need to edit the yaml file of prometheus
vi prometheus.yaml
#update the yaml file
- job_name: 'node_exporter'
static_configs:
- targets: ['13.233.151.45:9100']
- job_name: 'Jenkins'
metrics_path: '/prometheus'
static_configs:
- targets: ['13.233.151.45:8080']
After update the yaml file make sure restart the prometheus
pgrep prometheus
kill id
./prometheus &

Now in Grafana Create another dashboard for node exporter
import dashboard id - 1860


## How to Run
1. Run the application
2. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
3. You can also sign-up as a new user and customize your role to play with the application! 😊
