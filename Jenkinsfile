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
                dir('api-test'){
                    git credentialsId: 'githublogin', url: 'https://github.com/kcabarite/tasks-api-test'
                    sleep(20)
                    bat 'mvn test'
                }
            }
        }

        stage ('Deploy Frontend') {
            steps {
                dir('frontend'){
                    git credentialsId: 'githublogin', url: 'https://github.com/kcabarite/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }

        stage ('Funtional Test') {
            steps {
                dir('funtional-test'){
                    git credentialsId: 'githublogin', url: 'https://github.com/kcabarite/-kcabarite-tasks-funcional-test'
                    bat 'mvn test'
                }
            }
        }
        stage ('Deploy Prod') {
            steps {
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }

    }
}

