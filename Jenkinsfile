pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('saiteja_jen_docker')
        IMAGE_NAME = "shaiksaiteja/finalsemproject:${env.BUILD_NUMBER}"
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/shaiksaiteja/IQAP.git', branch: 'main'
            }
        }

        stage('Build Docker img') {
            steps {
                script {
                    sh 'docker build -t shaiksaiteja/finalsemproject:$BUILD_NUMBER .'
                }
            }
        }

        stage('LOGIN TO DOCKERHUB') {
            steps {
                script {
                    sh 'docker --version'
                    withCredentials([usernamePassword(credentialsId: 'saiteja_jen_docker', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                        sh "docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
                    }
                }
            }
        }

        stage('PUSH IMAGE') {
            steps {
                script {
                    sh 'docker push shaiksaiteja/finalsemproject:$BUILD_NUMBER'
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
