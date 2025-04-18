pipeline {
    agent any

    environment {
        // DockerHub 저장소
        REPOSITORY = "jeonghyuck/jenkins-test"

        // Jenkins에서 등록한 DockerHub 크레덴셜
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-jenkins')

        // GitOps 저장소용 SSH 크레덴셜
        GIT_CREDENTIALS_ID = 'jenkins-ssh-private'
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $REPOSITORY:$BUILD_NUMBER .'
                    //sh 'docker tag $REPOSITORY:$BUILD_NUMBER $REPOSITORY:latest'
                }
            }
        }

        stage('Docker Hub 로그인') {
            steps {
                script {
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

        stage('GitOps Deploy Commit') {
            steps {
                sshagent(credentials: [env.GIT_CREDENTIALS_ID]) {
                    script {
                        sh '''
                            set -e
                            export GIT_SSH_COMMAND="ssh -oStrictHostKeyChecking=no"

                            // git clone git@github.com:cure4itches/docker-hello-world-deployment.git
                            git clone git@github.com:jeong-github/test-git.git
                            cd test-git
                            git config user.email "cure4itches@gmail.com"
                            git config user.name "cure4itches"

                            # sed를 사용하여 이미지 태그 변경
                            sed -i "s|jeonghyuck/jenkins-test:[a-zA-Z0-9._-]*|jeonghyuck/jenkins-test:${BUILD_NUMBER}|" deploy.yaml

                            git add .
                            git commit -m "Update image tag to ${BUILD_NUMBER}"
                            git push origin main
                        '''
                    }
                }
            }
        }


        stage('Clean Up Docker Image') {
            steps {
                script {
                    sh 'docker rmi $REPOSITORY:$BUILD_NUMBER || true'
                    sh 'docker rmi $REPOSITORY:latest || true'
                }
            }
        }
    }
}

