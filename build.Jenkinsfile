pipeline {
    agent any

    environment {
        // Set your Docker registry URL
        DOCKER_REGISTRY = "gershonmx"

        // Set your Docker image name
        DOCKER_IMAGE_NAME = "my_roberta"

        // Set your Docker image tag (e.g., version number, commit hash, etc.)
        DOCKER_IMAGE_TAG = "latest"
    }

    stages {
        stage('Build') {
            steps {
                    // Log in to Docker registry
                    sh "docker login ${DOCKER_REGISTRY}"

                    // Build Docker image
                    sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."

                    // Tag Docker image
                    sh "docker tag ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"

                    // Push Docker image to registry
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
        }
    }
}

