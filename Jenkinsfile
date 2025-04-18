pipeline {
    agent any

    environment {
        // Docker Hub ID와 레포지토리 이름 (예: jeonghyuck/jenkins-test)
        REPOSITORY = "jeonghyuck/jenkins-test"

        // Jenkins에 미리 등록한 Docker Hub 자격 증명 ID
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-jenkins')
    }

    stages {
        stage('Clone Source') {
            steps {
                echo "Git 또는 파일 복사를 여기에 작성하세요"
                // git 'https://github.com/yourname/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Dockerfile이 현재 디렉토리에 존재해야 함
                    sh 'docker build -t $REPOSITORY:$BUILD_NUMBER .'
                    //sh 'docker build -t jeonghyuck/jenkins-test:version2 .'
                }
            }
        }

        stage('Docker Hub 로그인') {
            steps {
                script {
                    // Jenkins Credentials Plugin 사용
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Docker 이미지 푸시') {
            steps {
                script {
                    sh 'docker push $REPOSITORY:$BUILD_NUMBER'
                }
            }
        }

      	stage('Trigger 2nd Job') {
            steps {
                echo 'Trigger 2nd Job...'
                build job: 'pipeline-for-manifest', wait: true
            }
        }
     
        
    }
}
