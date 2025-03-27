pipeline {
    agent any
    environment {
        IMAGE_NAME = "bhadaneunnati1110/node-app"  // DockerHub username: sakshi2105
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/UnnatiBhadane/NodeA.git',
                    branch: 'main',
                   
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${env.BUILD_ID}").inside {
                        sh 'node --version'
                    }
                }
            }
        }
        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push()
                        docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push('latest')
                    }
                }
            }
        }
stage('Deploy') {
            steps {
                script {
                    sh 'docker stop node-app || true && docker rm node-app || true'
                    sh "docker run -d --name node-app -p 3000:3000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }
    post {
        always {
            sh 'docker system prune -f'
        }
    }
}
