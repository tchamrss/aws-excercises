
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

        stage('Deploy') {
    steps {
        echo 'Deploying application to EC2...'

        sshagent(credentials: ['ec2-server-key']) {
            sh '''
                set -eu

                EC2_HOST="ec2-user@51.20.108.4"
                DEPLOY_DIR="/home/ec2-user/aws-exercises"

                ssh -o StrictHostKeyChecking=accept-new \
                    "$EC2_HOST" \
                    "mkdir -p $DEPLOY_DIR"

                scp -o StrictHostKeyChecking=accept-new \
                    -r app docker-compose.yml \
                    "$EC2_HOST:$DEPLOY_DIR/"

                ssh -o StrictHostKeyChecking=accept-new \
                    "$EC2_HOST" \
                    "cd $DEPLOY_DIR && docker compose up -d --build"
            '''
        }
    }
}
    }
}
