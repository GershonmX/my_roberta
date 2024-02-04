pipeline {
    agent any
    parameters {string(name: 'ROBERTA_IMAGE_URL', defaultValue: '', description: '')}

    options {
        timestamps()
    }

    stages {
        stage('Deploy') {
            steps {
            '''
                // complete this code to deploy to real k8s cluster
                sh '# kubectl apply -f ...'
                sh 'echo $ROBERTA_IMAGE_URL'
            '''
            }
        }
    }
}