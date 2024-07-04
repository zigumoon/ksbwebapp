pipeline {
    agent none
    
<<<<<<< HEAD
    stages {
        stage('Git Checkout') {
            agent any
=======
    triggers {
      // SCM 폴링 트리거: 매 분마다 SCM에서 변경 사항을 확인합니다.
        pollSCM('* * * * *')
        }

    stages {
        stage('Checkout') {
          // GitHub 저장소에서 main 브랜치를 체크아웃합니다.
>>>>>>> 41e4845b1e93fdefe76ffe0e29ec9ca67ed5f8c2
            steps {
                git branch: 'main', 
                url: 'https://github.com/zigumoon/ksbwebapp'
            }
        }

<<<<<<< HEAD
        stage('Maven Build') {
            agent {
                docker 'maven:3-openjdk-17'
            }
=======
        stage('Build') {
          // Maven을 사용하여 빌드를 수행하고, 테스트 단계를 생략합니다.
>>>>>>> 41e4845b1e93fdefe76ffe0e29ec9ca67ed5f8c2
            steps {
                sh 'mvn package -Dmaven.test.skip=true'
            }
        }
<<<<<<< HEAD

        stage('Maven Test') {
            agent {
                docker 'maven:3-openjdk-17'
            }
=======
        stage('Test') {
          // Maven을 사용하여 테스트를 수행합니다.
>>>>>>> 41e4845b1e93fdefe76ffe0e29ec9ca67ed5f8c2
            steps {
                sh 'mvn test'
            }
        }
<<<<<<< HEAD

        stage('Build Docker Image') {
            agent any
            steps {
                sh 'docker image build -t ksbwebapp .'      
=======
        stage('Deploy') {
          // Tomcat 서버에 WAR 파일을 배포합니다.
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-manager', url: 'http://192.168.56.102:8080/')], contextPath: null, war: 'target/hello-world.war'
>>>>>>> 41e4845b1e93fdefe76ffe0e29ec9ca67ed5f8c2
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
