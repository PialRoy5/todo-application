pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/PialRoy5/todo-application.git'
        DOCKER_IMAGE = 'pialroy5/todo-application-image:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'master', url: GITHUB_REPO
                }
            }
        }
        
        stage('Build and Push Docker') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USER -p $DOCKER_PASSWORD"
                    }
                }
                sh "docker build -t ${DOCKER_IMAGE} ."
                sh "docker push ${DOCKER_IMAGE}"
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker compose down || exit 0"
                sh "docker compose up -d"
            }
        }
        
        stage('Verify') {
            steps {
                sh "docker ps"
            }
        }
        
        stage("Clean") {
            steps {
                sh "rm -rf *"
            }
        }
    }
}