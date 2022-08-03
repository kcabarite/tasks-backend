pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
         stage ('Unit Test') {
            steps {
                bat 'mvn test'
            }
        }

          stage ('Deploy Backend') {
            steps {
                sleep(20)
                deploy adapters: [tomcat8(credentialsId: 'tomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

             stage ('API Test') {
            steps {
                git credentialsId: 'githublogin', url: 'https://github.com/kcabarite/tasks-api-test'
                sleep(20)
                bat 'mvn test'
            }
        }
    }
}

