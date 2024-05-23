pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-django-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/mvieraamado/django-app-BG'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build the image with Podman
                    sh "podman build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Stop and remove the previous container if it exists
                    sh """
                    podman stop ${IMAGE_NAME} || true
                    podman rm ${IMAGE_NAME} || true
                    """

                    // Run the new container
                    sh "podman run -d --name ${IMAGE_NAME} -p 8000:8000 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up intermediate containers and images
            sh "podman system prune -f"
        }
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}