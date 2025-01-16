pipeline {
    agent {
        docker {
            image 'docker:20.10.24'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nginx:latest .'
            }
        }
    }
}
