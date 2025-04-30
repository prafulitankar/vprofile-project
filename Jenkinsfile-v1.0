pipeline {
    agent any
    tools {
        maven  "maven3"
        jdk  "jdk17"
    }
    
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub' // Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE_NAME = 'stepstech/devops-weekend'
        DOCKER_TAG = 'latest' // You can use versioning or a unique tag as needed
        CONTAINER_NAME = "webapp-${env.BUILD_ID}"
        
    }

    stages {
        stage ('Fetching code') {
            steps {
                git branch: 'main', url: 'https://github.com/prafulitankar/vprofile-project.git' //for docker host
                
            }
        }

        stage ('Source code build'){
            steps {
                sh 'mvn install -DskipTests'  
            }

            post {
            success {  
                echo 'Archieving the Artifact'
                archiveArtifacts artifacts: '**/*.war'
            }
          }
        }

        stage ('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage ('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }

        }
        stage ('Sonar Scanner') {
            environment {
                scannerHome = tool 'Sonar4.7'
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh '''${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=vprofile \
                        -Dsonar.projectName=vprofile \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest  \
                        -Dsonar.junit.reportPaths=target/surefire-reports/ \
                        -Dsonar.jacoco.reportPaths=target/jacoco.exec \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }    
        }
        stage ("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                  waitForQualityGate abortPipeline: true
              }
            }
          }
        stage ("Artifact Uploader"){
            steps {
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '172.31.46.89:8081', // Do not use http here and we also use Private Ip of Nexus Server
                groupId: 'Test', // this is mendatory and value can be optional
                version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}", //Enable  BUILD_TIMESTAMP from System
                repository: 'vproject',
                credentialsId: 'nexuslogin',
                artifacts: [
                    [artifactId: 'vproapp',
                    classifier: '',
                    file: 'target/vprofile-v2.war',
                    type: 'war']
                ]
             )
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    //docker.build("${DOCKER_IMAGE_NAME}:${BUILD_ID}") // To version the Docker Images
                    docker.build("${DOCKER_IMAGE_NAME}:latest") // To provide the same tag i.e. latest 
                }
            }
        }
        //To remove Old Containers From Jenkins Server itself
        stage('List Running Docker Container and Remove') {
            steps {
                sh 'docker ps -q'
                echo "Running container having above ids"
                sh 'docker container stop $(docker ps -qa)'
                sh 'docker container rm $(docker ps -qa)'
                 
            }
        }
       //Run Docker Container on Jenkins Server
        stage('Build Docker Container') {
            steps {
                script {
                    // Run Docker container from image
                    docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").run('-p 8032:8080 --name ${CONTAINER_NAME}') 
                    //Container run with the latest tag and constant port i.e. 8012
                    //docker.image("${DOCKER_IMAGE_NAME}:${BUILD_ID}").run('-p 80${BUILD_ID}:8080 --name ${CONTAINER_NAME}') 
                    // Example run options: detached mode, port mapping In this command docker image is version and container run with different port i.e.80with build id 
                    
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        echo 'Logged in to Docker Hub'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").push("${DOCKER_TAG}") //push the docker images with tag latest
                        //docker.image("${DOCKER_IMAGE_NAME}:${BUILD_ID}").push("${BUILD_ID}") // push the docker images with the versioning
                        echo 'Docker Image Successfully Pushed to Docker Hub'
                    }
                }
            }
        }
        //Pull Docker Image on Remote Docker Host
        stage('Pull Docker Image on Remote Docker host') {
          steps {
            sshagent(['jenkins-docker-intergation']) { 
                sh "ssh -o StrictHostKeyChecking=no -l ubuntu 172.31.11.118  'docker pull $DOCKER_IMAGE_NAME:$DOCKER_TAG'"
                // jenkins-docker-intergation : add credential to connect docker host using Username with Private SSH key
                //Note : docker image pull by ubuntu user on remote docker host so ubuntu user must be part of docker group [usermod -aG  docker ubuntu]
                echo 'Docker Image Pulled on Remote Docker Host Successfully'
            }
         }
      }
        //Scan Docker Image on Remote Docker Host
        stage('Scan Docker Image by Trivy') {
          steps {
            sshagent(['jenkins-docker-intergation']) { 
                sh "ssh -o StrictHostKeyChecking=no -l ubuntu 172.31.11.118  'trivy image $DOCKER_IMAGE_NAME:$DOCKER_TAG'"
                // jenkins-docker-intergation : add credential to connect docker host using Username with Private SSH key
                echo 'Docker Image Scanned Successfully'
            }
         }
      }
        //To remove Old Containers on Remote docker host
        stage('List Running Docker Container and Remove on Remote Host') {
            steps {
               sshagent(['jenkins-docker-intergation']) { 
			   sh """ssh -o StrictHostKeyChecking=no -l ubuntu 172.31.11.118 '
			         docker ps -aq | xargs -r docker stop &&
                     docker ps -aq | xargs -r docker rm
               '
			   """
			   }
            }
        }
        //To Run Docker Container on Remote Docker Host
        stage('Build Docker Container on Remote Host') {
            steps {
                sshagent(['jenkins-docker-intergation']) {
                    // Run Docker container from image
                    //docker.image("${DOCKER_IMAGE_NAME}").run('-d -p 8006:8080 --name ${CONTAINER_NAME}') // Example run options: detached mode, port mapping
                    sh "ssh -o StrictHostKeyChecking=no -l ubuntu 172.31.11.118 'docker run -itd --name $CONTAINER_NAME -p 8020:8080 $DOCKER_IMAGE_NAME:$DOCKER_TAG'"
                    echo 'Docker Container Successfully run on Remote Docker Host'
                }
            }
        }
        
        stage('Deploying App to Kubernetes') {
            steps {
                script {
                  kubernetesDeploy(configs: "deploy.yaml", kubeconfigId: "kubernetes")
                }
           }
         }


     }
    
        post {
            success {
                echo 'Dockerised Application Deployed Successfully!'
        }
            failure {
                echo 'There was an issue with building Dockerised Application.'
        }
    }
}
