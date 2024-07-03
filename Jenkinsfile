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
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-manager', url: 'https://github.com/zigumoon/ksbwebapp')], contextPath: null, war: 'target/hello-world.war'
            }
        }
    }
}