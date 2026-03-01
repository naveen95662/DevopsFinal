pipeline {
    agent any

    environment {
        // REPLACE 'your-dockerhub-username' with your actual Docker Hub ID
        DOCKER_HUB_USER = 'naveen9566'
        IMAGE_NAME = 'node-capstone-app'
        // Uses the Jenkins Build Number as a unique tag
        IMAGE_TAG = "${env.BUILD_ID}"
        // This MUST match the Credential ID you created in Jenkins
        DOCKER_CREDS_ID = '1e4f0cbc-1ab3-414f-999b-c321cab9790f'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image: ${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                sh "docker tag ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS_ID}", passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo "Deploying Container..."
                // Stop and remove existing container if it exists to avoid port conflicts
                sh "docker stop ${IMAGE_NAME} || true"
                sh "docker rm ${IMAGE_NAME} || true"
                // Run the new container
                sh "docker run -d --name ${IMAGE_NAME} -p 3000:3000 ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully! App is live on port 3000."
        }
        failure {
            echo "Pipeline failed. Check the console logs for errors."
        }
    }
}
