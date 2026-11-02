pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devops-demo'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install --no-cache-dir -r app/requirements.txt
                    pytest -q
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                      -t ${IMAGE_NAME}:${IMAGE_TAG} \
                      ./app
                '''
            }
        }

        stage('Verify Image') {
            steps {
                sh 'docker image inspect ${IMAGE_NAME}:${IMAGE_TAG} >/dev/null'
            }
        }
    }

    post {
        success {
            echo 'Tests passed and Docker image built successfully.'
        }
        failure {
            echo 'Pipeline failed. Review the logs before deployment.'
        }
        always {
            cleanWs()
        }
    }
}
