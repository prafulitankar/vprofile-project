## Jenkins Installation
* Dependecnies : Openjdk-11/17
* OS : Ubuntu-22.04
* Port No. : 8080

``` groovy
#!/bin/bash
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install maven wget unzip -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
systemctl enable jenkins
```

