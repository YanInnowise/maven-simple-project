# Jenkins-Maven-Nexus(in Docker)



## Step 1 - Docker

Build docker-compose file whitch creating 3 containers (jenkins, nginx, sonatype nexus) and custum network on bridge driver, containers working in this network and all of them has static ip-addres on this network(#editing-this-readme)!

## Step 2 - Nexus

Open Sonatype-nexus using localhost:8081 or *.*.*.*:8081 (*.*.*.*- custom ip from Step 1) on your web-browser, create here repository called "maven", repo should has Mixed and Rewritable sett``

## Step 3 -Jenkins

Go to jenkins thw you went to nexus, with only different- port (:8080)
Install suggested pluggins then go to Manage jenkins/Manage pluggins and instal pluggins from the list below
Crate 3 jobs (Freestyle job, Maven project, Pipeline), in Global tool configuration set JDK and Maven settings (install automatically [last version])

### 1 Job
Type: Maven project
Name:jenkins-maven-build
General: Restrict where this project can be run (label:build)
Source Code Management: Git (project repo)
Build: Root POM (pom.xml)

### 2 Job
Type: Freestyle job
Name:jenkins-maven-deploy
General: Restrict where this project can be run (label:build)
Source Code Management: None
Build triggers:Build after other projects are built (jenkin-maven-build)
Build steps: - Copy artifacts from another project
     Project name (jenkin-maven-build)
     Which build (Copy grom WORKSPACE of latest compiled build)
     Artifact to copy (*.jar)
             - Nexus artifact uploader
             
### 3 Job
Type: Pipeline
Name:jenkins-maven-test
Build-triggers:Build after other projects are built (jenkin-maven-deploy)
Use Jenkinsfile

### Pluggins
1. Git
2. GitHub
3. Maven Integration
4. Copy Artifacts
5. SSh Agent
6. Locale
7. Nexus Artifact Uploader
8. Build Timestamp
9. Build Name and Description Setter
10. Pipeline Utility Steps
11. Role-based Authorization Strategy 

## Jenkins slaves on GCP

Go to GCP console and create 2 instances e2-micro 1Gb, allow HTTP and HTTPS, network tag - default-allow-icmp,connect to them using ssh.
On each of them:
Create user Jenkins
Install Java, set $JAVA_HOME
Generate ssh-key

Go to Jenkinsand create 2 Nodes, Slave1(build) and Slave2(test)
### Slave1 
Remote root directory -/home/jenkins/
Labels - build
Launch method - Launch via SSH
    Host -*GCP 1-instance external ip*
    Credentials - use SSH private key frome instance-1, user: jenkins, (advice:fill the ID and description)
    Host Key Verification Strategy - Manually tusted
### Slave2
Remote root directory - /home/jenkins/
Labels - build
Launch method - Launch via SSH
    Host -*GCP 2-instance external ip*
    Credentials - use SSH private key frome instance-1, user: jenkins, (advice:fill the ID and description)
    Host Key Verification Strategy - Manually tusted
## Run
Run build jenkins-maven-build< check nexus repo - here you have to see nexus mrtadata with artifacts
(artifats name need to be like *verstion*-*date*time*)

