# Jenkins
Install Jenkins, Configure Docker, Setting up CI/CD, Deploy applications to k8s.

# Followig steps were followed by referring to informative video by abhishekveeramalla https://www.youtube.com/watch?v=zZfhAXfBvVA&list=PLdpzxOOAlwvIKMhk8WhzN1pYoJ1YU8Csa&index=23  

### STEP 1 : CREATING AN AWS EC2 INSTANCE.
Need to go to AWS Console > Instances(running) > Click on the Launch instance button > Fill in all the required details for configuring the instance > Click Launch Instance
![alt text](image1.png)

### STEP 2 : Install Jenkins.
Pre-Requisite:
 - Java(JDK)

### STEP 3 : Follwoing commands need to be executed to install Java and Jenkins.
- Install Java
--- sudo apt update
--- sudo apt install openjdk-17-jre 

- Install Jenkins 
 --- sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
(https://www.jenkins.io/doc/book/installing/linux/)

- Verify if java is installed by "java -version".

Point to Ponder : Jenkins won't be accessible from external world due to restrictions imposed by AWS on the inbound traffic. Hence the port 8080 needs to be opened by following the below steps : 
    - EC2 > Instances > Click on the instance.
    - Click on security tab from the below list.
    - Followed by opening the Security groups.
    - Add inbound traffic rules and allow traffic on the port 8080. You may either allow TCP on 8080 or allow all traffic.
    ![inbound rules](image2.png)

- Login to Jenkins using URL "http://<Public IP_address of the EC2 instance>:8080

- After login to Jenkins you may need to execute the command "sudo cat /var/lib/jenkins/secrets/initialAdminPassword" and enter the administartor password .
![admin passowrd](image3.png)

- Need to install the suggested packages by clicking on Install suggested plugins.
![Installing required plugins](image4.png)

- Create First Admin User
![admin user](image5.png)

- On succesfull installation of the Jenkins we may start using it.
![Jenkins](image6.png)

### STEP 4 : DOCKER Configuration
Run the below command to Install Docker :
    - sudo apt update
    - sudo apt install docker.io

### STEP 5 : Granting Jenkins user and Ubuntu user permission to docker deamon.
    sudo su - 
    usermod -aG docker jenkins
    usermod -aG docker ubuntu
    systemctl restart docker

After above steps are performed do restart the Jenkins.
http://<ec2-instance-public-ip>:8080/restart

Succesfully the Docker Agent has been configured.

### STEP 6 : Installing the Docker Pipeline plugin in Jenkins:

    - Log in to Jenkins.
    - Go to Manage Jenkins > Manage Plugins.
    - In the Available tab, search for "Docker Pipeline".
    - Select the plugin and click the Install button.
    - Restart Jenkins after the plugin is installed.