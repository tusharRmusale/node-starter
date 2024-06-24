pipeline {
    agent any
    
    tools {
        nodejs 'Latest node'  // Ensure this matches the name in Global Tool Configuration
    }

    stages {
        stage('Clone Repository') {
            steps {
               sh 'git pull origin main'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                sh '''

                # Stop the existing application
                pm2 stop my-app || true

                # Start the new application
                pm2 start dist/main.js --name my-app

                # Save the pm2 process list and corresponding environments
                pm2 save
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
