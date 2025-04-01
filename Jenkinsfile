pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = 'bhadaneunnati1110'  // Updated to your username
        APP_NAME = 'node-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-credentials', 
                    url: 'https://github.com/UnnatiBhadane/NodeA.git'
                sh 'ls -la'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_USERNAME}/${APP_NAME}:${env.BUILD_ID}")
                    sh 'docker images'
                }
            }
        }
        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhubcredentials') {
                        def appImage = docker.image("${DOCKERHUB_USERNAME}/${APP_NAME}:${env.BUILD_ID}")
                        appImage.push()
                        appImage.push('latest')
                    }
                    sh 'docker images'
                }
            }
        }
        stage('Clean Up') {
            steps {
                sh "docker rmi ${DOCKERHUB_USERNAME}/${APP_NAME}:${env.BUILD_ID}"
                sh "docker rmi ${DOCKERHUB_USERNAME}/${APP_NAME}:latest"
            }
        }
    }
    post {
        success { echo "Pipeline completed successfully!" }
        failure { echo "Pipeline failed." }
    }
}
