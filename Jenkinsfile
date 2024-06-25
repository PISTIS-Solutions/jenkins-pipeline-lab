pipeline {
    agent any

    environment {
        EC2_IP =  credential('EC2_IP') // Store EC2 instance IP as a Jenkins credential
        SSH_USER = credentials('SSH_USER')        // Store SSH username as a Jenkins credential
        SSH_KEY = credentials('SSH_KEY')          // Store SSH private key as a Jenkins credential
        HTML_FILE_PATH = 'index.html'  // Path to your local HTML file
    }

    stages {
        stage('Install Apache') {
            steps {
                script {
                    // Use SSH credentials to connect and execute commands on the EC2 instance
                    sh """
                    ssh -o StrictHostKeyChecking=no -i "${SSH_KEY}" ${SSH_USER}@${EC2_IP} << EOF
                    sudo yum update -y
                    sudo yum install httpd -y
                    sudo systemctl start httpd
                    sudo systemctl enable httpd
                    EOF
                    """
                }
            }
        }

        stage('Deploy HTML') {
            steps {
                script {
                    // Read HTML content from the local file
                    def htmlContent = readFile(file: HTML_FILE_PATH).trim()

                    // Deploy HTML content to the Apache web server on the EC2 instance
                    sh """
                    ssh -o StrictHostKeyChecking=no -i "${SSH_KEY}" ${SSH_USER}@${EC2_IP} << EOF
                    echo '${htmlContent}' | sudo tee /var/www/html/index.html
                    EOF
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

