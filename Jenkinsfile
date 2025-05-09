pipeline {
    agent any

    tools {
        jdk 'jdk21' // Ensure JDK 21 is set up in Jenkins tools
    }

    environment {
        IMAGE_NAME = 'bharani2121/e-commerce-devops-project' // Docker image name
        TAG = 'latest' // Docker image tag
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir()  // Clean workspace before starting the build
            }
        }

        stage('Git Checkout') {
            steps {
                // Clone the repository from GitHub
                git branch: 'main',
                    credentialsId: 'bharani_github_cred',  // Add your GitHub credentials here
                    url: 'https://github.com/bharani2121/DEVOPS_PROJECT_DEPLOY.git'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Using DockerHub credentials to authenticate and push the image
                    withDockerRegistry([credentialsId: 'bharani_dockerhub_cred']) {
                        // Docker build, tag, and push steps
                        sh """
                            docker build -t ${IMAGE_NAME} .
                            docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${TAG}
                            docker push ${IMAGE_NAME}:${TAG}
                        """
                    }
                }
            }
        }
    }
}
