
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is checked out by Jenkins.'
            }
        }

        stage('Test') {
            steps {
                echo 'Building test image...'
                sh '''
                    docker build \
                    -f app/Dockerfile.test \
                    -t bootcamp-nodejs-test:$BUILD_NUMBER \
                    app
                '''

                echo 'Running Node.js tests...'
                sh 'docker run --rm bootcamp-nodejs-test:$BUILD_NUMBER'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                sh 'docker build -t bootcamp-nodejs-app:$BUILD_NUMBER ./app'
            }
        }

        // Deployment ergänzen wir, sobald SSH eingerichtet ist.
    }
}
