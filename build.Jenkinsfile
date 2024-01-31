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
                        // Handle any exceptions, e.g., send notification or mark build as unstable
                        currentBuild.result = 'FAILURE'
                        echo "Error: ${e.message}"
                    } finally {
                        // Always clean up, even if there was an exception
                        sh "docker logout"
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up steps that should run regardless of success or failure
            script {
                // Additional cleanup steps if needed
            }
        }

        success {
            // Notify on successful build
            echo "Docker image built and pushed successfully."
            // Add additional success notifications if needed
        }

        failure {
            // Notify on build failure
            echo "Build failed. Check the logs for details."
            // Add additional failure notifications if needed
        }
    }
}
