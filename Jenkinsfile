pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/partizang2/source-maven-java-spring-hello-webapp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                // 플러그인 대신 직접 curl 명령어로 배포
                withCredentials([usernamePassword(credentialsId: 'tomcat', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "curl -u ${USER}:${PASS} --upload-file target/hello-world.war 'http://192.168.56.102:8080/manager/text/deploy?path=/hello-world&update=true'"
                }
            }
        }
    }
}
