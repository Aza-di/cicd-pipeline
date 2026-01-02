pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Aza-di/cicd-pipeline.git'
            }
        }

        stage('Application Build') {
            steps {
                sh './build.sh'
            }
        }

        stage('Tests') {
            steps {
                sh './test.sh'
            }
        }

        stage('Docker Image Build') {
            steps {
                sh 'docker build -t azat279/my-app:latest .'
            }
        }

        stage('Docker Image Push') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-token', variable: 'DOCKER_TOKEN')]) {
                    sh '''
                    echo $DOCKER_TOKEN | docker login -u azat279 --password-stdin
                    docker push azat279/my-app:latest
                    '''
                }
            }
        }
    }
}

