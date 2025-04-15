pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'jeonghyuck'
        IMAGE = 'jeonghyuck/jenkins-test:latest'
        CREDS_ID = 'dockerhub-jenkins'
    }
    stages {
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', CREDS_ID) {
                        docker.build(IMAGE).push()
                    }
                }
            }
        }
    }
}
