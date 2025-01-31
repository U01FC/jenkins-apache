pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh '''
                        echo "Cloning the GitHub repository..."
                        git clone https://github.com/U01FC/jenkins-apache.git ~/09-Jenkins || echo "Repository is cloned"
                    '''
                }
            }
        }

        stage('Install Apache2') {
            steps {
                script {
                    sh '''
                        echo "Installing Apache2..."
                        sudo apt update
                        sudo apt install -y apache2
                        sudo systemctl start apache2
                        sudo systemctl enable apache2
						echo "Apache2 is installed"
                    '''
                }
            }
        }

        stage('Check Apache Logs for Errors') {
            steps {
                script {
                    def errorLog = sh(script: "sudo grep -E 'HTTP/1.[01]\" [45][0-9][0-9]' /var/log/apache2/access.log || echo 'No errors found'", returnStdout: true).trim()

                    if (errorLog && !errorLog.contains("No errors found")) {
                        echo "Found HTTP 4xx/5xx errors in logs:"
                        echo errorLog
                    } else {
                        echo "No HTTP 4xx/5xx errors found in logs."
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed."
        }
    }
}
