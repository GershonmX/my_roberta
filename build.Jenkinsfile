pipeline {
    agent any

    environment {
        // Set your Docker registry URL
        DOCKER_REGISTRY = "gershonmx"

        // Set your Docker image name
        DOCKER_IMAGE_NAME = "my_roberta"

        // Set your Docker image tag (e.g., version number, commit hash, etc.)
        DOCKER_IMAGE_TAG = "0.0.$BUILD_NUMBER"
    }

    stages {
        stage('Build') {
            steps {
                    withCredentials([usernamePassword(credentialsId: 'gershonmx-dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                    // Log in to Docker registry
                    sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                    // Build Docker image
                    sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:0.0.$DOCKER_IMAGE_TAG ."
                    // Push Docker image to registry
                    sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:0.0.$DOCKER_IMAGE_TAG"
                }
        }
    }
}

