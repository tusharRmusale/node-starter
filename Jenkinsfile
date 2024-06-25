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
            mail to: 'robinhooda66@gmail.com,xdankitjain@gmail.com,pallavisnaikdigital@gmail.com',
                 subject: "Build Success: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "The build ${env.JOB_NAME} ${env.BUILD_NUMBER} was successful."
        }
        failure {
            mail to: 'robinhooda66@gmail.com',
                 subject: "Build Failure: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "The build ${env.JOB_NAME} ${env.BUILD_NUMBER} failed."
        }
    }
}
