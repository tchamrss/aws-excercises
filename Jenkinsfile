
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
        echo 'Checking project files...'
        sh '''
            set -eu
            pwd
            ls -la app
            test -f app/package-lock.json
        '''

        echo 'Running Node.js tests...'
        sh '''
            docker run --rm \
              -v "$WORKSPACE/app:/app" \
              -w /app \
              node:22-alpine \
              sh -c "npm ci && npm test"
        '''
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
