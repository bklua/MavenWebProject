### Assignment ###  
![alt text](https://github.com/jasonltr/KCMavenWebProject/blob/master/Images/Screenshot%20from%202022-04-25%2015-06-51.png)  

## installing jenkins ##  
[guide to install jenkins](https://www.jenkins.io/doc/book/installing/linux/)  
`java -version` to check java version  
`systemctl start jenkins`  
localhost:8080, user:admin, pass: 
Jenkins Dashboard ->Manage Jenkins ->Manage plugins ->Available ->Maven Integration ->Install.  
install deploy to container plugin for tomcat  [link](https://www.middlewareinventory.com/blog/jenkins-tomcat-deploy-deploying-application-tomcat-using-jenkins/)  


## installing sonarqube (pull docker image and run that) ##  
[guide to starting sonarqube on localhost](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/)  
[sonarqube image on docker](https://hub.docker.com/_/sonarqube/)  
`docker pull sonarqube`  
`docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest`  
`docker ps`  
localhost:9000  

## installing tomcat ##  
[installing tomcat9](https://linuxhint.com/install_apache_tomcat_server_ubuntu/)  
[installing jdk8](http://openjdk.java.net/install/index.html)  
`sudo apt-get install openjdk-8-jre`  
`sudo nano /etc/tomcat9/server.xml` to change port number as 8080 was already in use by jenkins  
`sudo nano /etc/tomcat9/tomcat-users.xml` to add and modify user credentials  
`systemctl start tomcat9`
localhost:8082  

## installing nexus ##  
[starting nexus on localhost](https://ahgh.medium.com/how-to-setup-sonatype-nexus-3-repository-manager-using-docker-7ff89bc311ce)  
`docker pull sonatype/nexus`  
`docker run -d -p 8081:8081 --name nexus sonatype/nexus3`
`docker ps`
`docker container exec nexus cat nexus-data/admin.password`  
localhost:8081  
created maven2(hosted) repo name:testmaven-snapshot  

## install maven ##  
`sudo apt update`  
`sudo apt install maven`  

## git repo ##  
[repo](https://github.com/jasonltr/KCMavenWebProject)  
[tutorial](https://www.youtube.com/watch?v=meaD9y1RPNc)  

## Jenkins configuration ##  
dashboard -> new item -> maven project -> add github repo link  
global tool configuration -> add java file path in JDK  
global tool configuration -> add maven file path in maven  
dashboard -> add plugin -> deploy to container plugin (for tomcat9)
configure project -> post-build actions -> deploy war/ear to container -> target/*.war  
containers -> tomcat credentials from  `/etc/tomcat9/tomcat-users.xml` ->tomcat9.x remote -> tomcat url  

## Groovy file ##  
Create new pipeline (different from the maven project above)
Start with maven template  
Use the Jenkins Snippte Generator to mimic some of the plugins (sonarqube,nexus artifact uploader, deploy to container(for tomcat9))  
For Sonarqube syntax, use [Jenkins Doc](https://www.jenkins.io/doc/pipeline/steps/sonar/)  
For Nexus3 Repo use [this](https://www.youtube.com/watch?v=ftTjxztcT14)[Jenkins Doc](https://www.jenkins.io/doc/pipeline/steps/nexus-artifact-uploader/)  
For Tomcat9 use jenkins snippet generator for deploying into container (tomcat9) [Jenkins Doc](https://www.jenkins.io/doc/pipeline/steps/deploy/#deploy-deploy-warear-to-a-container)  

### Output ###  
Groovy file can be found [here](https://github.com/jasonltr/KCMavenWebProject/blob/master/pipeline)  
![pipeline](https://github.com/jasonltr/KCMavenWebProject/blob/master/Images/pipeline%20success.jpg)  
![sonarqube](https://github.com/jasonltr/KCMavenWebProject/blob/master/Images/Sonarqube%20success.png)  
![nexus3](https://github.com/jasonltr/KCMavenWebProject/blob/master/Images/nexus3%20success.png)  
![tomcat9](https://github.com/jasonltr/KCMavenWebProject/blob/master/Images/tomcat9%20success.png)  

### main tutorials followed ###
[Video1](https://www.youtube.com/watch?v=XSgs0VBEI8c)  
[Video2](https://www.youtube.com/watch?v=meaD9y1RPNc)  