pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'pialroy5/todo-application-image:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'git-credentials', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        sh "git clone https://${GIT_USER}:${GIT_PASS}@github.com/PialRoy5/todo-application.git"
                        sh "cd todo-application"
                    }
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
                sh "docker build -t ${DOCKER_IMAGE} todo-application"
                sh "docker push ${DOCKER_IMAGE}"
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker compose -f todo-application/docker-compose.yml down || exit 0"
                sh "docker compose -f todo-application/docker-compose.yml up -d"
            }
        }
        
        stage('Verify') {
            steps {
                sh "docker ps"
            }
        }
        
        stage("Clean") {
            steps {
                sh "rm -rf todo-application"
            }
        }
    }
}