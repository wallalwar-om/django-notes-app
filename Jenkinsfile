@Library("Shared") _
pipeline {
    agent { label 'Agent-1' }

    environment {
        IMAGE_NAME = "notes-app"
        CONTAINER_NAME = "notes-app-container"
        PORT = "8000"
    }

    stages {
        stage("Hello") {
            steps {
                script {
                    echo "Groovy files name"
                    hello()
                }
            }
        }
        stage("Code clone") {
            steps {
                script {
                    clone("https://github.com/wallalwar-om/django-notes-app.git", "dev")
                }
            }
        }

        stage("Code Build") {
            steps {
                script {
                    docker_build(IMAGE_NAME, "latest")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    docker_push(IMAGE_NAME, "latest", "omwallalwar")
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying container..."
                sh """
                docker rm -f ${CONTAINER_NAME} || true
                docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${PORT}:${PORT} \
                    ${IMAGE_NAME}:latest
                """
            }
        }
    }
}
