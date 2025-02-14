pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-cicd-app"
        CONTAINER_NAME = "flask-app-container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/harshini1203/task-6-flask-cicd.git'
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
                    sh 'docker run -d -p 5000:5000 --name $CONTAINER_NAME $IMAGE_NAME'
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
