pipeline {
    agent any  // Runs on any available agent (server)

    environment {
        // Define environment variables like AWS credentials, region, etc.
        AWS_REGION = 'us-west-2'
        EC2_INSTANCE_IP = 'your-ec2-public-ip'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the GitHub repo
                git branch: 'main', url: 'git@github.com:your-username/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                // Build the application (e.g., Maven for Java projects)
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
        }

        stage('Deploy to AWS') {
            steps {
                // Deploy the application to AWS EC2 instance
                sshagent(['aws-ssh-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/myapp.jar ec2-user@${EC2_INSTANCE_IP}:/home/ec2-user/
                    ssh -o StrictHostKeyChecking=no ec2-user@${EC2_INSTANCE_IP} 'sudo systemctl restart myapp'
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up resources, or notify users
            echo 'Pipeline completed'
        }
        success {
            // Actions on success (e.g., notify on Slack, send email)
            echo 'Build and Deploy successful'
        }
        failure {
            // Actions on failure (e.g., notify on Slack, send email)
            echo 'Pipeline failed'
        }
    }
}
