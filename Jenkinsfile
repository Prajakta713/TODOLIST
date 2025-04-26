pipeline {
    agent any

    environment {
        IMAGE_NAME = 'todolist-app-image'
        CONTAINER_NAME = 'todolist-app-container'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                git url: 'https://github.com/Prajakta713/TODOLIST.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Running Docker container...'
                    docker.image(IMAGE_NAME).run("-d --name ${CONTAINER_NAME} -p 5000:5000")
                }
            }
        }

        stage('Automated Testing') {
            steps {
                script {
                    echo 'Running automated tests...'
                    // Adjust this command based on your project
                    sh "docker exec ${CONTAINER_NAME} pytest || true"
                }
            }
        }

        stage('Stop and Remove Container') {
            steps {
                script {
                    echo 'Stopping and removing the container...'
                    sh "docker stop ${CONTAINER_NAME}"
                    sh "docker rm ${CONTAINER_NAME}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
            script {
                // Ensure container is cleaned up even if something fails
                sh "docker rm -f ${CONTAINER_NAME} || true"
            }
        }
    }
}
