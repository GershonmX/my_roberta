pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "gershonmx"
        DOCKER_IMAGE_NAME = "my_roberta"
        DOCKER_IMAGE_TAG = "0.0.${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}"
        IMAGE_TAG = "${IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
    }

    options {
        timestamps()
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                // Log in to Docker registry
                withCredentials([usernamePassword(credentialsId: 'gershonmx-dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "docker login -u${USERNAME} -p${PASSWORD}"
                }

                // Build Docker image
                sh "docker build -t ${IMAGE_TAG} ."

                // Push Docker image to registry
                sh "docker push ${IMAGE_TAG}"
            }
        }

        stage('Trigger Deploy') {
            steps {
                script {
                    // Trigger the deploy job with the specified parameters
                    build job: 'RobertaDeploy', wait: false, parameters: [
                        string(name: 'ROBERTA_IMAGE_URL', value: "${IMAGE_TAG}:${DOCKER_IMAGE_TAG}")
                    ]
                }
            }
        }

        stage('Test') {
            steps {
                // Integrate Snyk security scanning
                snykSecurity(
                    snykInstallation: 'snyk-gershonm',
                    snykTokenId: 'gershon-snyk'
                    // place other parameters here
                )
            }
        }
    }

    post {
        always {
            // Cleanup steps, e.g., logout from Docker registry and prune
            sh "docker logout"
            sh "docker image prune -a --force --filter until=24h"
            sh "docker container prune --force --filter until=24h"
            sh "docker system prune --force --filter until=24h"
            cleanWs()
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


