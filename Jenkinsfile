pipeline {
    agent any

    environment {
        EC2_IP         = credentials('EC2_INSTANCE_IP') // Store EC2 instance IP as a Jenkins credential
        SSH_USER       = credentials('SSH_USER')        // Store SSH username as a Jenkins credential
        SSH_KEY        = credentials('SSH_KEY')         // Store SSH private key as a Jenkins credential
        HTML_FILE_PATH = 'index.html'                   // Path to your local HTML file
    }

    stages {
        stage('Compile Stage') {
            steps {
                echo 'Compiling the code...'
                // Add any compilation steps here if necessary
            }
        }

        stage('Build Stage') {
            steps {
                script {
                    // Use SSH credentials to connect and execute commands on the EC2 instance
                    sh """
ssh -o StrictHostKeyChecking=no -i "${SSH_KEY}" ${SSH_USER}@${EC2_IP} << 'EOF'
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
EOF
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Read HTML content from the local file
                    def htmlContent = readFile(file: HTML_FILE_PATH).trim()

                    // Deploy HTML content to the Apache web server on the EC2 instance
                    sh """
ssh -o StrictHostKeyChecking=no -i "${SSH_KEY}" ${SSH_USER}@${EC2_IP} << 'EOF'
echo '${htmlContent}' | sudo tee /var/www/html/index.html
EOF
                    """
                }
            }
        }

        stage('Deploy MySQL') {
            steps {
                script {
                    // Install MySQL on the EC2 instance
                    sh """
ssh -o StrictHostKeyChecking=no -i "${SSH_KEY}" ${SSH_USER}@${EC2_IP} << 'EOF'
sudo apt update -y
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
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
