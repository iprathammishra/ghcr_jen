pipeline {
    agent any

    environment {
        IMAGE_NAME = "ghcr.io/iprathammishra/flask-ci-demo"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/iprathammishra/ghcr_jen.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh '''
                        docker build -t ${IMAGE_NAME}:latest .
                    '''
                }
            }
        }

        stage('Login to GHCR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ghcr_credentials', usernameVariable: 'USER', passwordVariable: 'TOKEN')]) {
                    sh '''
                        echo $TOKEN | docker login ghcr.io -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                    docker push ${IMAGE_NAME}:latest
                '''
            }
        }
    }
}
