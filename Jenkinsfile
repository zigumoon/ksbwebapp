pipeline {
    agent any
    
    triggers {
      // SCM 폴링 트리거: 매 분마다 SCM에서 변경 사항을 확인합니다.
        pollSCM('* * * * *')
        }

    stages {
        stage('Checkout') {
          // GitHub 저장소에서 main 브랜치를 체크아웃합니다.
            steps {
                git branch: 'main', 
                url: 'https://github.com/zigumoon/ksbwebapp'
            }
        }

        stage('Build') {
          // Maven을 사용하여 빌드를 수행하고, 테스트 단계를 생략합니다.
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }
        stage('Test') {
          // Maven을 사용하여 테스트를 수행합니다.
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
          // Tomcat 서버에 WAR 파일을 배포합니다.
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-manager', url: 'http://192.168.56.102:8080/')], contextPath: null, war: 'target/hello-world.war'
            }
        }
    }
}
