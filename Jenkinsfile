pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/gyenoch/web-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t gyenoch/web-app:latest ."
            } 
        }

        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'Docker-cred') {
                        sh "docker push gyenoch/web-app:latest"
                    }
                }
            }
        }

        stage('Docker Run') {
            steps {
                sh "docker run -d -p 9090:8080 gyenoch/web-app:latest"
            }
        }
    }
}