pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx-index-check"
        CONTAINER_NAME = "nginx-test"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                  docker rm -f $CONTAINER_NAME || true
                  docker run -d --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

        stage('Verify index.html exists') {
            steps {
                sh '''
                  docker exec $CONTAINER_NAME ls -l /usr/share/nginx/html/
                  docker exec $CONTAINER_NAME test -f /usr/share/nginx/html/index.html
                '''
            }
        }
    }

    post {
        success {
            echo "Docker image built, container running, index.html verified"
        }

        always {
            sh '''
              docker rm -f $CONTAINER_NAME || true
            '''
        }
    }
}
