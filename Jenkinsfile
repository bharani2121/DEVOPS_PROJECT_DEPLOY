pipeline {
    agent any

    tools {
        jdk 'jdk21'
    }

    environment {
        IMAGE_NAME = 'bharani2121/e-commerce-devops-project'
        TAG = 'latest'
        PORT = '8090' // Changed to avoid conflict with Jenkins
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'bharani_github_cred',
                    url: 'https://github.com/bharani2121/DEVOPS_PROJECT_DEPLOY.git'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'dockerhub-credentials']) {
                        sh """
                            docker build -t ${IMAGE_NAME} .
                            docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${TAG}
                            docker push ${IMAGE_NAME}:${TAG}
                        """
                    }
                }
            }
        }

        stage('Run Docker Container Locally') {
            steps {
                script {
                    sh """
                        docker stop devops_html_container || true
                        docker rm devops_html_container || true
                    """

                    dockerRunCmd = """
                        docker run -d -p ${PORT}:80 --name devops_html_container ${IMAGE_NAME}:${TAG}
                    """
                    sh dockerRunCmd

                    echo "Your HTML app is running at: http://localhost:${PORT}"
                }
            }
        }
    }
}
