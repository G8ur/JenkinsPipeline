pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "mern-demo"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/G8ur/JenkinsPipeline.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building Backend Docker Image..."
                    sh 'docker build -t ${DOCKER_IMAGE_NAME}-backend ./backend'
                    echo "Building Frontend Docker Image..."
                    sh 'docker build -t ${DOCKER_IMAGE_NAME}-frontend ./frontend'
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                script {
                    echo "Running Docker containers..."
                    sh '''
                    docker rm -f mern-backend || true
                    docker rm -f mern-frontend || true
                    docker run -d -p 5000:5000 --name mern-backend ${DOCKER_IMAGE_NAME}-backend
                    docker run -d -p 3000:80 --name mern-frontend ${DOCKER_IMAGE_NAME}-frontend
                    '''
                }
            }
        }
    }
}
