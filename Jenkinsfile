pipeline {
    agent {
        label 'worker02'  // Docker가 설치된 외부 노드 지정
    }

    environment {
        DOCKERHUB_USER = 'jeonghyuck'
        IMAGE_NAME = 'jenkins-test'
        IMAGE_TAG = 'latest'
        CREDS_ID = 'dockerhub-jenkins'
    }

    stages {
        stage('Docker Build and Push') {
            steps {
                script {
                    // Docker 소켓 마운트가 이미 되어 있어야 함
                    docker.withRegistry('https://index.docker.io/v1/', CREDS_ID) {
                        docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}

