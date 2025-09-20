**Wanderlust - Your Ultimate Travel Blog 🌍✈️**

WanderLust is a simple MERN travel blog website ✈ This project is aimed
to help people to contribute in open source, upskill in react and also
master git.

<img src="/mnt/data/assets/media/image1.png"
style="width:6.26667in;height:4.36667in" alt="Preview Image" />

**Wanderlust Mega Project End to End Implementation**

**In this demo, we will see how to deploy an end to end three tier MERN
stack application on EKS cluster.**

**Project Deployment Flow:**

<img src="/mnt/data/assets/media/image2.GIF"
style="width:6.26667in;height:3.53333in"
alt="A diagram of a software company AI-generated content may be incorrect." />

**Tech stack used in this project:**

-   GitHub (Code)

-   Docker (Containerization)

-   Jenkins (CI)

-   OWASP (Dependency check)

-   SonarQube (Quality)

-   Trivy (Filesystem Scan)

-   ArgoCD (CD)

-   Redis (Caching)

-   AWS EKS (Kubernetes)

-   Helm (Monitoring using grafana and prometheus)

**How pipeline will look after deployment:**

-   **CI pipeline to build and push** 

> <img src="/mnt/data/assets/media/image3.png"
> style="width:6.275in;height:2.875in"
> alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **CD pipeline to update application version** 

> <img src="/mnt/data/assets/media/image4.png"
> style="width:6.26667in;height:3.28333in"
> alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **ArgoCD application for deployment on
    > EKS** <img src="/mnt/data/assets/media/image5.png"
    > style="width:6.275in;height:3.33333in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

Important

Below table helps you to navigate to the particular tool installation
section fast.

| **Tech stack**           | **Installation**                                                                                                                               |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Jenkins Master           | [<u>Install and configure Jenkins</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Jenkins)                    |
| eksctl                   | [<u>Install eksctl</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#EKS)                                       |
| Argocd                   | [<u>Install and configure ArgoCD</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Argo)                        |
| Jenkins-Worker Setup     | [<u>Install and configure Jenkins Worker Node</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Jenkins-worker) |
| OWASP setup              | [<u>Install and configure OWASP</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Owasp)                        |
| SonarQube                | [<u>Install and configure SonarQube</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Sonar)                    |
| Email Notification Setup | [<u>Email notification setup</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Mail)                            |
| Monitoring               | [<u>Prometheus and grafana setup using helm charts</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Monitor)   |
| Clean Up                 | [<u>Clean up</u>](https://github.com/shubhs11patil/Wanderlust-Mega-Project?tab=readme-ov-file#Clean)                                           |

**Pre-requisites to implement this project:**

Note

This project will be implemented on North California region (us-west-1).

-   **Create 1 Master machine on AWS with 2CPU, 8GB of RAM (t2.large)
    > and 29 GB of storage and install Docker on it.**

<!-- -->

-   **Open the below ports in security group of master machine and also
    > attach same security group to Jenkins worker node (We will create
    > worker node
    > shortly)** <img src="/mnt/data/assets/media/image6.png"
    > style="width:6.26667in;height:2.575in" alt="image" />

Note

We are creating this master machine because we will configure Jenkins
master, eksctl, EKS cluster creation from here.

Install & Configure Docker by using below command, "NewGrp docker" will
refresh the group config hence no need to restart the EC2 machine.

sudo apt-get update

sudo apt-get install docker.io -y

sudo usermod -aG docker ubuntu && newgrp docker

-   **Install and configure Jenkins (Master machine)**

sudo apt update -y

sudo apt install fontconfig openjdk-17-jre -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \\

https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb \[signed-by=/usr/share/keyrings/jenkins-keyring.asc\]" \\

https://pkg.jenkins.io/debian-stable binary/ \| sudo tee \\

/etc/apt/sources.list.d/jenkins.list \> /dev/null

sudo apt-get update -y

sudo apt-get install jenkins -y

-   **Now, access Jenkins Master on the browser on port 8080 and
    > configure it**.

<!-- -->

-   **Create EKS Cluster on AWS (Master machine)**

    -   IAM user with **access keys and secret access keys**

    -   AWSCLI should be configured ([<u>Setup
        > AWSCLI</u>](https://github.com/DevMadhup/DevOps-Tools-Installations/blob/main/AWSCLI/AWSCLI.sh))

-   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o
    > "awscliv2.zip"

-   sudo apt install unzip

-   unzip awscliv2.zip

-   sudo ./aws/install

aws configure

-   Install **kubectl** (Master machine)([<u>Setup
    > kubectl </u>](https://github.com/DevMadhup/DevOps-Tools-Installations/blob/main/Kubectl/Kubectl.sh))

curl -o kubectl
https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin

kubectl version --short --client

-   Install **eksctl** (Master machine) ([<u>Setup
    > eksctl</u>](https://github.com/DevMadhup/DevOps-Tools-Installations/blob/main/eksctl /eksctl.sh))

curl --silent --location
"https://github.com/weaveworks/eksctl/releases/latest/download/eksctl\_$(uname
-s)\_amd64.tar.gz" \| tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version

-   **Create EKS Cluster (Master machine)**

eksctl create cluster --name=wanderlust \\

--region=us-east-2 \\

--version=1.30 \\

--without-nodegroup

-   **Associate IAM OIDC Provider (Master machine)**

eksctl utils associate-iam-oidc-provider \\

--region us-east-2 \\

--cluster wanderlust \\

--approve

-   **Create Nodegroup (Master machine)**

eksctl create nodegroup --cluster=wanderlust \\

--region=us-east-2 \\

--name=wanderlust \\

--node-type=t2.large \\

--nodes=2 \\

--nodes-min=2 \\

--nodes-max=2 \\

--node-volume-size=29 \\

--ssh-access \\

--ssh-public-key=eks-nodegroup-key

Note

Make sure the ssh-public-key "eks-nodegroup-key is available in your aws
account"

-   **Setting up jenkins worker node**

    -   Create a new EC2 instance (Jenkins Worker) with 2CPU, 8GB of RAM
        > (t2.large) and 29 GB of storage and install java on it

-   sudo apt update -y

sudo apt install fontconfig openjdk-17-jre -y

-   Create an IAM role with administrator access attach it to the
    > jenkins worker node Select Jenkins worker node EC2 instance --\>
    > Actions --\> Security --\> Modify IAM
    > role <img src="/mnt/data/assets/media/image7.png"
    > style="width:6.275in;height:2.65in" alt="image" />

-   Configure AWSCLI ([<u>Setup
    > AWSCLI</u>](https://github.com/DevMadhup/DevOps-Tools-Installations/blob/main/AWSCLI/AWSCLI.sh))

sudo su

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o
"awscliv2.zip"

sudo apt install unzip

unzip awscliv2.zip

sudo ./aws/install

aws configure

-   **generate ssh keys (Master machine) to setup jenkins master-slave**

ssh-keygen

<img src="/mnt/data/assets/media/image8.png"
style="width:6.26667in;height:3.26667in" alt="image" />

-   **Now move to directory where your ssh keys are generated and copy
    > the content of public key and paste to authorized_keys file of the
    > Jenkins worker node.**

<!-- -->

-   **Now, go to the jenkins master and navigate to Manage jenkins --\>
    > Nodes, and click on Add node**

    -   **name:** Node

    -   **type:** permanent agent

    -   **Number of executors:** 2

    -   Remote root directory

    -   **Labels:** Node

    -   **Usage:** Only build jobs with label expressions matching this
        > node

    -   **Launch method:** Via ssh

    -   **Host:** \<public-ip-worker-jenkins\>

    -   **Credentials:** Add --\> Kind: ssh username with private key
        > --\> ID: Worker --\> Description: Worker --\> Username: root
        > --\> Private key: Enter directly --\> Add Private key

    -   **Host Key Verification Strategy:** Non verifying Verification
        > Strategy

    -   **Availability:** Keep this agent online as much as possible

<!-- -->

-   And your jenkins worker node is
    > added <img src="/mnt/data/assets/media/image9.png"
    > style="width:6.275in;height:2.25833in" alt="image" />

<!-- -->

-   **Install docker (Jenkins Worker)**

sudo apt install docker.io -y

sudo usermod -aG docker ubuntu && newgrp docker

-   **Install and configure SonarQube (Master machine)**

docker run -itd --name SonarQube-Server -p 9000:9000
sonarqube:lts-community

-   **Install Trivy (Jenkins Worker)**

sudo apt-get install wget apt-transport-https gnupg lsb-release -y

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key \|
sudo apt-key add -

echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release
-sc) main \| sudo tee -a /etc/apt/sources.list.d/trivy.list

sudo apt-get update -y

sudo apt-get install trivy -y

-   **Install and Configure ArgoCD (Master Machine)**

    -   **Create argocd namespace**

kubectl create namespace argocd

-   **Apply argocd manifest**

kubectl apply -n argocd -f
https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

-   **Make sure all pods are running in argocd namespace**

watch kubectl get pods -n argocd

-   **Install argocd CLI**

sudo curl --silent --location -o /usr/local/bin/argocd
https://github.com/argoproj/argo-cd/releases/download/v2.4.7/argocd-linux-amd64

-   **Provide executable permission**

sudo chmod +x /usr/local/bin/argocd

-   **Check argocd services**

kubectl get svc -n argocd

-   **Change argocd server's service from ClusterIP to NodePort**

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type":
"NodePort"}}'

-   **Confirm service is patched or not**

kubectl get svc -n argocd

-   **Check the port where ArgoCD server is running and expose it on
    > security groups of a worker node** 

-   <img src="/mnt/data/assets/media/image10.png"
    > style="width:6.275in;height:1.51667in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **Access it on browser, click on advance and proceed with**

\<public-ip-worker\>:\<port\>

  <img src="/mnt/data/assets/media/image11.png"
style="width:6.25833in;height:3.26667in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **Fetch the initial password of argocd server**

kubectl -n argocd get secret argocd-initial-admin-secret -o
jsonpath="{.data.password}" \| base64 -d; echo

-   **Username: admin**

-   **Now, go to User Info and update your argocd password**

**Steps to add email notification**

-   **Go to your Jenkins Master EC2 instance and allow 465 port number
    > for SMTPS**

<!-- -->

-   **Now, we need to generate an application password from our gmail
    > account to authenticate with jenkins**

    -   **Open gmail and go to Manage your Google Account --\>
        > Security**

Important

**Make sure 2 step verification must be on**

<img src="/mnt/data/assets/media/image12.png"
style="width:6.26667in;height:2.95833in" alt="image" />

-   **Search for App password and create a app password for
    > jenkins **<img src="/mnt/data/assets/media/image13.png"
    > style="width:6.26667in;height:2.725in" alt="image" />** **<img src="/mnt/data/assets/media/image14.png"
    > style="width:6.26667in;height:2.8in" alt="image" />

<!-- -->

-   **Once, app password is create and go back to jenkins Manage Jenkins
    > --\> Credentials to add username and password for email
    > notification **

-   <img src="/mnt/data/assets/media/image15.png"
    > style="width:6.275in;height:3.325in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

<!-- -->

-   **Go back to Manage Jenkins --\> System and search for Extended
    > E-mail
    > Notification **<img src="/mnt/data/assets/media/image16.png"
    > style="width:6.26667in;height:3.08333in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

<!-- -->

-   **Scroll down and search for E-mail Notification and setup email
    > notification**

Important

**Enter your gmail password which we copied recently in password
field E-mail Notification --\> Advance**

<img src="/mnt/data/assets/media/image17.png"
style="width:6.26667in;height:3.06667in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

**  **

**Steps to implement the project:**

-   **Go to Jenkins Master and click on Manage Jenkins --\> Plugins --\>
    > Available plugins install the below plugins:**

    -   **OWASP**

    -   **SonarQube Scanner**

    -   **Docker**

    -   **Pipeline: Stage View**

<!-- -->

-   **Configure OWASP, move to Manage Jenkins --\> Plugins --\>
    > Available plugins (Jenkins
    > Worker) **<img src="/mnt/data/assets/media/image18.png"
    > style="width:6.26667in;height:2.96667in" alt="image" />

-   **After OWASP plugin is installed, Now move to Manage jenkins --\>
    > Tools (Jenkins
    > Worker) **<img src="/mnt/data/assets/media/image19.png"
    > style="width:6.26667in;height:3.19167in" alt="image" />

<!-- -->

-   **Login to SonarQube server and create the credentials for jenkins
    > to integrate with SonarQube**

    -   **Navigate to Administration --\> Security --\> Users --\>
        > Token **<img src="/mnt/data/assets/media/image20.png"
        > style="width:6.275in;height:2.625in" alt="image" />** **<img src="/mnt/data/assets/media/image21.png"
        > style="width:6.26667in;height:1.49167in" alt="image" />** **<img src="/mnt/data/assets/media/image22.png"
        > style="width:6.26667in;height:1.825in" alt="image" />

<!-- -->

-   **Now, go to Manage Jenkins --\> credentials and add Sonarqube
    > credentials: **<img src="/mnt/data/assets/media/image23.png"
    > style="width:6.26667in;height:2.925in" alt="image" />

<!-- -->

-   **Go to Manage Jenkins --\> Tools and search for SonarQube Scanner
    > installations: **<img src="/mnt/data/assets/media/image24.png"
    > style="width:6.26667in;height:2.93333in" alt="image" />

<!-- -->

-   **Go to Manage Jenkins --\> credentials and add Github credentials
    > to push updated code from the
    > pipeline: **<img src="/mnt/data/assets/media/image25.png"
    > style="width:6.26667in;height:3.03333in" alt="image" />

Note

**While adding github credentials add Personal Access Token in the
password field.**

-   **Go to Manage Jenkins --\> System and search for SonarQube
    > installations: **<img src="/mnt/data/assets/media/image26.png"
    > style="width:6.275in;height:2.66667in" alt="image" />

<!-- -->

-   **Now again, Go to Manage Jenkins --\> System and search for Global
    > Trusted Pipeline
    > Libraries:\</b **<img src="/mnt/data/assets/media/image27.png"
    > style="width:6.26667in;height:3.00833in" alt="image" />** **<img src="/mnt/data/assets/media/image28.png"
    > style="width:6.26667in;height:2.875in" alt="image" />

<!-- -->

-   **Login to SonarQube server, go to Administration --\> Webhook and
    > click on create **<img src="/mnt/data/assets/media/image29.png"
    > style="width:6.26667in;height:2.9in" alt="image" />** **<img src="/mnt/data/assets/media/image30.png"
    > style="width:5.175in;height:5.66667in" alt="image" />

<!-- -->

-   **Now, go to github repository and under Automations directory
    > update the instance-id field on both the updatefrontendnew.sh
    > updatebackendnew.sh with the k8s worker's instance
    > id **<img src="/mnt/data/assets/media/image31.png"
    > style="width:6.25833in;height:3.40833in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

<!-- -->

-   **Navigate to Manage Jenkins --\> credentials and add credentials
    > for docker login to push docker image: **

-   <img src="/mnt/data/assets/media/image32.png"
    > style="width:6.26667in;height:3.325in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

<!-- -->

-   **Create
    > a Wanderlust-CI pipeline **<img src="/mnt/data/assets/media/image33.png"
    > style="width:6.26667in;height:2.85in" alt="image" />

<!-- -->

-   **Create one more
    > pipeline Wanderlust-CD **<img src="/mnt/data/assets/media/image34.png"
    > style="width:6.26667in;height:2.775in" alt="image" />** **<img src="/mnt/data/assets/media/image35.png"
    > style="width:6.26667in;height:3.03333in" alt="image" />** **<img src="/mnt/data/assets/media/image36.png"
    > style="width:6.26667in;height:2.84167in" alt="image" />

<!-- -->

-   **Provide permission to docker socket so that docker build and push
    > command do not fail (Jenkins Worker)**

**chmod 777 /var/run/docker.sock**

-   **Go to Master Machine and add our own eks cluster to argocd for
    > application deployment using cli**

    -   **Login to argoCD from CLI**

**argocd login 18.117.249.10:30444 --username admin**

Tip

**18.117.249.10:30444 --\> This should be your argocd url**

<img src="/mnt/data/assets/media/image37.png"
style="width:6.26667in;height:0.91667in" alt="image" />

-   **Check how many clusters are available in argocd**

**argocd cluster list**

<img src="/mnt/data/assets/media/image38.png"
style="width:6.26667in;height:0.74167in" alt="image" />

-   **Get your cluster name**

**kubectl config get-contexts**

<img src="/mnt/data/assets/media/image39.png"
style="width:6.26667in;height:0.84167in" alt="image" />

-   **Add your cluster to argocd**

**argocd cluster add Wanderlust@wanderlust.us-west-1.eksctl.io --name
wanderlust-eks-cluster**

Tip

**[<u>Wanderlust@wanderlust.us-west-1.eksctl.io</u>](mailto:Wanderlust@wanderlust.us-west-1.eksctl.io) --\>
This should be your EKS Cluster Name.**

<img src="/mnt/data/assets/media/image40.png"
style="width:6.26667in;height:1.05in" alt="image" />

-   **Once your cluster is added to argocd, go to argocd
    > console Settings --\> Clusters and verify it **

-   <img src="/mnt/data/assets/media/image41.png"
    > style="width:6.26667in;height:1.85833in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

<!-- -->

-   **Go to Settings --\> Repositories and click on Connect
    > repo **<img src="/mnt/data/assets/media/image42.png"
    > style="width:6.25833in;height:3.325in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />** **<img src="/mnt/data/assets/media/image43.png"
    > style="width:6.26667in;height:2.78333in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

-   ** **<img src="/mnt/data/assets/media/image44.png"
    > style="width:6.26667in;height:1.95in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

Note

**Connection should be successful**

-   **Now, go to Applications and click on New App**

<img src="/mnt/data/assets/media/image45.png"
style="width:6.26667in;height:2.54167in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

Important

**Make sure to click on the Auto-Create Namespace option while creating
argocd application**

<img src="/mnt/data/assets/media/image46.png"
style="width:6.26667in;height:2.19167in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

** **<img src="/mnt/data/assets/media/image47.png"
style="width:6.26667in;height:2.26667in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **Congratulations, your application is deployed on AWS EKS
    > Cluster **<img src="/mnt/data/assets/media/image48.png"
    > style="width:6.26667in;height:3.01667in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />** **<img src="/mnt/data/assets/media/image49.png"
    > style="width:6.275in;height:3.35in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **Open port 31000 and 31100 on worker node and Access it on
    > browser**

**\<worker-public-ip\>:31000**

<img src="/mnt/data/assets/media/image50.png"
style="width:6.25833in;height:3.25in"
alt="A screenshot of a computer AI-generated content may be incorrect." />**  **<img src="/mnt/data/assets/media/image51.png"
style="width:6.26667in;height:1.70833in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

-   **Email Notification **<img src="/mnt/data/assets/media/image52.png"
    > style="width:6.26667in;height:3.71667in"
    > alt="A screenshot of a computer AI-generated content may be incorrect." />

**How to monitor EKS cluster, kubernetes components and workloads using
prometheus and grafana via HELM (On Master machine)**

-   **Install Helm Chart**

**curl -fsSL -o get_helm.sh
https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3**

**chmod 700 get_helm.sh**

**./get_helm.sh**

-   **Add Helm Stable Charts for Your Local Client**

**helm repo add stable https://charts.helm.sh/stable**

-   **Add Prometheus Helm Repository**

**helm repo add prometheus-community
https://prometheus-community.github.io/helm-charts**

-   **Create Prometheus Namespace**

**kubectl create namespace prometheus**

**kubectl get ns**

-   **Install Prometheus using Helm**

**helm install stable prometheus-community/kube-prometheus-stack -n
prometheus**

-   **Verify prometheus installation**

**kubectl get pods -n prometheus**

-   **Check the services file (svc) of the Prometheus**

**kubectl get svc -n prometheus**

-   **Expose Prometheus and Grafana to the external world through Node
    > Port**

Important

**change it from Cluster IP to NodePort after changing make sure you
save the file and open the assigned nodeport to the service.**

**kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus**

<img src="/mnt/data/assets/media/image53.png"
style="width:5.09167in;height:4.13333in" alt="image" />** **<img src="/mnt/data/assets/media/image54.png"
style="width:5.78333in;height:0.21667in" alt="image" />

-   **Verify service**

**kubectl get svc -n prometheus**

-   **Now,let’s change the SVC file of the Grafana and expose it to the
    > outer world**

**kubectl edit svc stable-grafana -n prometheus**

<img src="/mnt/data/assets/media/image55.png"
style="width:4.98333in;height:3.75833in" alt="image" />

-   **Check grafana service**

**kubectl get svc -n prometheus**

-   **Get a password for grafana**

**kubectl get secret --namespace prometheus stable-grafana -o
jsonpath="{.data.admin-password}" \| base64 --decode ; echo**

Note

**Username: admin**

-   **Now, view the Dashboard in
    > Grafana **<img src="/mnt/data/assets/media/image56.png"
    > style="width:6.26667in;height:3.11667in" alt="image" />** **<img src="/mnt/data/assets/media/image57.png"
    > style="width:6.275in;height:3.10833in" alt="image" />** **<img src="/mnt/data/assets/media/image58.png"
    > style="width:6.26667in;height:3.14167in" alt="image" />** **<img src="/mnt/data/assets/media/image59.png"
    > style="width:6.26667in;height:3.15833in" alt="image" />

**Clean Up**

-   **Delete eks cluster**

**eksctl delete cluster --name=wanderlust --region=us-west-1**
