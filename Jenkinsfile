pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-cicd-app"
        CONTAINER_NAME = "flask-app-container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloned successfully'
            }
        }
    stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-token', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh '''
                        docker stop flask-app-container || true
                        docker rm flask-app-container || true
                        docker run -d -p 5000:5000 --name flask-app-container harshini1203/flask-cicd-app
                        echo "Checking running containers..."
                        docker ps
                    '''
                }
            }
        }

     

        stage('Cleanup Previous Containers') {
            steps {
                script {
                    sh 'docker ps -a -q --filter "name=$CONTAINER_NAME" | grep -q . && docker stop $CONTAINER_NAME && docker rm $CONTAINER_NAME || true'
                }
            }
        }
    }

    post {
        always {
            sh 'docker images'
        }
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed'
        }
    }
}
