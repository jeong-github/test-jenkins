pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'nginx'
        DOCKER_REGISTRY = 'jeonghyuck/jenkins-test'
        REGISTRY_CREDENTIALS = 'dockerhub-jenkins'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Git에서 소스 코드 가져오기
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Docker 이미지를 빌드
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }
        
        stage('Push to Docker Registry') {
            steps {
                // Docker 레지스트리에 이미지 푸시
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", "${REGISTRY_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }
