pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
        }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/zigumoon/ksbwebapp'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: '<credentialsId>', url: 'http://192.168.56.102:8080/')], contextPath: null, war: 'target/hello-world.war'
            }
        }
    }
}