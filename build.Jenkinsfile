pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "gershonmx"
        DOCKER_IMAGE_NAME = "my_roberta"
        DOCKER_IMAGE_TAG = "0.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'gershonmx-dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            // Log in to Docker registry
                            sh "docker login -u${USERNAME} -p${PASSWORD}"
                            // Build Docker image
                            sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
                            // Push Docker image to registry
                            sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        echo "Error: ${e.message}"
                    } finally {
                        sh "docker logout"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Additional cleanup steps if needed
            }
        }

        success {
            echo "Docker image built and pushed successfully."
            // Add additional success notifications if needed
        }

        failure {
            echo "Build failed. Check the logs for details."
            // Add additional failure notifications if needed
        }
    }
}
