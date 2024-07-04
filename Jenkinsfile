pipeline {
    agent none
    
    stages {
        stage('Git Checkout') {
            agent any
            steps {
                git branch: 'main', 
                url: 'https://github.com/zigumoon/ksbwebapp'
            }
        }

        stage('Maven Build') {
            agent {
                docker 'maven:3-openjdk-17'
            }
            steps {
                sh 'mvn package -Dmaven.test.skip=true'
            }
        }

        stage('Maven Test') {
            agent {
                docker 'maven:3-openjdk-17'
            }
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            agent any
            steps {
                sh 'docker image build -t ksbwebapp .'      
            }
        }

        stage('Docker Image Tag') {
            agent any
            steps {
                sh 'docker image tag ksbwebapp zigumoon/ksbwebapp:latest'
                sh 'docker image tag ksbwebapp zigumoon/ksbwebapp:$BUILD_NUMBER'
            }
        }

        stage('Doker Image Push to Docker Hub') {
            agent any
            steps {
                withDockerRegistry(credentialsId: 'docker-hub-tocken', 
                    url: 'https://index.docker.io/v1/') {
                    sh 'docker image push ksbwebapp zigumoon/ksbwebapp:latest'
                    sh 'docker image push ksbwebapp zigumoon/ksbwebapp:$BUILD_NUMBER'
                }
            }
        }

        stage('Docker Container Deploy to Docker Out of Docker') {
            agent {
                docker { image 'docker:dind' } 
            }
            steps {
                sh  'docker -H tcp://192.168.56.104:2375 container run --
detach --name ksbwebapp -p 80:8080 zigumoon/ksbwebapp:$BUILD_NUMBER'
            }
        }

    
    }
}