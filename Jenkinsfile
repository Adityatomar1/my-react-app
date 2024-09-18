pipeline {
    agent any
    
    environment {
        // Define the Node.js version
        NODE_VERSION = '16.x'
    }

    stages {
        // Stage 1: Checkout the Code
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // Stage 2: Build the React App
        stage('Build') {
            steps {
                script {
                    // Install Node.js
                    sh 'curl -sL https://deb.nodesource.com/setup_${NODE_VERSION} | sudo -E bash -'
                    sh 'sudo apt-get install -y nodejs'
                    
                    // Install project dependencies
                    sh 'npm install'

                    // Build the React app
                    sh 'npm run build'
                }
            }
        }

        // Stage 3: Test the React App
        stage('Test') {
            steps {
                script {
                    // Run tests using the default "npm test"
                    sh 'npm test -- --watchAll=false'
                }
            }
        }

        // Stage 4: Deploy the App (optional step)
        stage('Deploy') {
            steps {
                script {
                    // Assuming the deployment is done by copying files to a server
                    // Here you can use scp, rsync, or any other deployment mechanism
                    
                    // Example: Use SCP to transfer files to a remote server
                    sh '''
                    scp -r ./build/* user@your-server:/var/www/html/
                    '''
                }
            }
        }
    }

    post {
        always {
            // Archive the build for future reference
            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
        }
        
        success {
            // Send notification on success (can be integrated with email or Slack)
            echo 'Build Successful!'
        }
        
        failure {
            // Send notification on failure
            echo 'Build Failed!'
        }
    }
}
