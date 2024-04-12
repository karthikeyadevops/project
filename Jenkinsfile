pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = 'nginx'
        TAG = env.BUILD_ID
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'public.ecr.aws/e9k0p2n3/jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your version control system (e.g., Git)
                git 'https://github.com/karthikkasani09/project.git'
            }
        }

        stage('Unit Tests') {
            steps {
                // Run unit tests
                sh 'npm test' // Replace with your test command
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image with a specific tag
                script {
                    docker.build("${nginx}:${latest}")
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                // Push Docker image to AWS ECR
                script {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'AWS_IAM_ACCESS_KEY',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        docker.withRegistry("https://${'533267398752'}.dkr.ecr.${us-east-1}.amazonaws.com", 'ecr:us-east-1') {
                            dockerImage.push("${public.ecr.aws/e9k0p2n3/jenkins}:${latest}")
                        }
                    }
                }
            }
        }
    }
}
