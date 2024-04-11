pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git 'https://github.com/karthikkasani09/projects.git'
                script {
                    docker.build("nodejs-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Test') {
            steps {
                sh 'npm install' // Install dependencies
                sh 'npm test'    // Run unit tests
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://public.ecr.aws/e9k0p2n3/jenkins.dkr.ecr.us-east-1.amazonaws.com', 'ecr:jenkins') {
                        docker.image("nodejs-app:${env.BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sh 'ssh -i ubuntu@54.81.87.125 "docker pull public.ecr.aws/e9k0p2n3/jenkins.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:latest"'
                sh 'ssh ubuntu@54.81.87.125 "docker run -d -p 80:3000 public.ecr.aws/e9k0p2n3/jenkins.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:latest"'
            }
        }
        stage('Security Configuration') {
            steps {
                sh 'ssh ubuntu@54.81.87.125 "sudo ufw allow 80"' // Allow HTTP traffic
            }
        }
        stage('Validation') {
            steps {
                sh 'curl http://54.81,87.125' // Validate the deployed application
            }
            post {
                success {
                    echo 'Validation successful!'
                }
                failure {
                    error 'Validation failed!'
                }
            }
        }
    }
    post {
        always {
            emailext subject: 'CI/CD Pipeline Report',
                     body: "Pipeline execution status: ${currentBuild.currentResult}",
                     to: 'your-email@example.com'
        }
    }
}

