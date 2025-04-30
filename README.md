# Prerequisites
#
- JDK 11 
- Maven 3 
- MySQL 8

# Technologies 
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- Tomcat
- MySQL
- Memcached
- Rabbitmq
- ElasticSearch
# Database
Here,we used Mysql DB 
sql dump file:
- /src/main/resources/db_backup.sql
- db_backup.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < db_backup.sql
  >
#Kubernetes Integration:
 - Install Plugins , Note: kubernetes-cd need to Install Manually
 - Download the Plugins Link : https://github.com/prafulitankar/vprofile-project/blob/main/kubernetes-cd.hpi
 - Jenkins Server : Manage Jenkins--> plugins --> Advance Setting --> Browse the Downloaded Plugins --> Install
 - Jenkins --> Kubernetes Integration : After Installation of kubernetes-cd you find the Kubernetes Configuration (kubeconfig) inside the    credentials mention id and description --> kubeconfig option : select enter directly option (Go to K8s Master run  cd ~; cd .kube ;       cat config) copy all the content from config file and paste into kubeconfig section
 - uplod deploy.yaml in github repository 


