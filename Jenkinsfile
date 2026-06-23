pipeline {

    agent any

    environment {
        IMAGE_NAME = 'manjula/nature-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                    echo $DOCKER_PASS | docker login \
                    -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop nature-app || true
                docker rm nature-app || true

                docker run -d \
                --name nature-app \
                -p 80:80 \
                $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}



























}
