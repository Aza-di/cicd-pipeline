pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Application Build') {
            steps {
                sh 'scripts/build.sh'
            }
        }

        stage('Tests') {
            steps {
                sh 'scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t azat279/my-app:latest .'
            }
        }

        stage('Push Docker Image') {
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

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully! Docker image pushed!'
        }
        failure {
            echo 'Pipeline failed — check the logs.'
        }
    }
}
