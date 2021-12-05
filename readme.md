# Link: https://docs.bitnami.com/tutorials/create-ci-cd-pipeline-jenkins-gke/
# 1. Install docker
curl -fsSL https://get.docker.com/ | sh
# 2. Install docker jenkin with port 8181 and mount
docker run -u 0 --privileged --name jenkins -it -d -p 8181:8080 -p 50000:50000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
-v /home/jenkins_home:/var/jenkins_home \
jenkins/jenkins:latest
# 3. Install plugin in jenkins
Docker Pipeline
Docker Plugin
Google Kubernetes Engine Plugin
Kubernetes CLI Plugin
Kubernetes plugin
GitHub Pull Request Builder
NodeJS Plugin
# 4. Add credential for docker, github token
Credential->System-Global credential->Add Credential
# 4.1 Add key docker hub
Kind: secret text
Secret: password docker
ID&Description: dockerhub
# 4.2 Add token github 
Kind: secret text
Secret: token of github
ID&Description: token-github
# 4.3 Create token github
# 4.4 Add key GKE
Click the "Add credentials" link. Select the "Google Service Account from private key" credential type and set the project name (which doubles as the credential identifier) to gke. Select the "JSON key" radio button and upload the JSON key obtained in Step 2. Click "OK" to save the changes.
repo, admin:org_hook, notifications, user, write:discussion 
# 5. Add NodeJS Plugin  "tools {nodejs "node"}" in Jenkinsfile
Global Tool Configuration -> NodeJS -> name: node
# 6. Add key token-github: 
configuration -> GitHub -> Credentials: select token-github -> Tick Manage hooks
# 7. Create item Pipeline
Tick "Github project" and add url of github
Tick "GitHub hook trigger for GITScm polling"
Pipeline: Definition -> Pipeline Script from SCM: https://github.com/chungndinh/demo-aks-k8s.git
# 8. Create service account (IAM & admin)
IAM & Admin -> Service Account -> Create and add Role Kubernetes Engine Admin -> Create key JSON
# 9. Install kubernetes in docker Jenkins
docker exec -it jenkins /bin/bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo 'deb http://apt.kubernetes.io/ kubernetes-xenial main' > /etc/apt/sources.list.d/kubernetes.list
apt-get update
apt-get install kubectl -y

