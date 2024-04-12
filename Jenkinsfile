pipeline {
    agent any

    environment {
        // Define environment variables
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'public.ecr.aws/e9k0p2n3/jenkins'
        DOCKER_IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/karthikkasani09/project.git'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh 'npm test' // Replace with your test command
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image and tag it with build version
                    docker.build("${ECR_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push Docker image to AWS ECR
                    withCredentials([awsEcr(credentialsId: 'AWS_IAM_ACCESS_KEY', region: "${AWS_REGION}")]) {
                        docker.withRegistry("https://${533267398752}.dkr.ecr.${AWS_REGION}.amazonaws.com", 'ecr:us-east-1') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
