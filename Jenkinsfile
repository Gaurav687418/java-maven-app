pipeline {
    agent any
    
    tools {
        maven "Maven3"
    }
    stages {
        stage('Clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Gaurav687418/java-maven-app.git'
            }
        }
        stage('maven war file build') {
            steps {
               sh 'mvn clean package'
            }
        }
        stage('Docker images/conatiner remove') {
            steps {
                script{
                        sh '''docker stop javamavenapp_container
                        docker rm javamavenapp_container
                        docker rmi javamavenapp gauravpai/javamavenapp:latest'''
                }  
            }
        }
        stage('Docker images - Push to dockerhub') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker', toolname: 'docker'){
                
                        sh '''docker build -t javamavenapp .
                        docker tag javamavenapp gauravpai/javamavenapp:latest
                        docker push  gauravpai/javamavenapp:latest'''
                      } 
                }
            }
        }
        stage('docker container of app') {
            steps {
               sh 'docker run -d -p 9000:8080 --name javamavenapp_container -t gauravpai/javamavenapp:latest'
            }
        }
        
    }
    
}
