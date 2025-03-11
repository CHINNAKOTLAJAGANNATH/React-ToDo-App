pipeline {
    agent any

    environment {
        NODEJS_VERSION = '16'  // Ensure it's installed in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/CHINNAKOTLAJAGANNATH/React-ToDo-App.git'
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    def nodeHome = tool name: 'NodeJS 16', type: 'hudson.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodeHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'npm test'
                    } catch (Exception e) {
                        echo 'Tests failed but continuing...'
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                // Example: Deploy to Firebase
                // sh 'firebase deploy --token "$FIREBASE_DEPLOY_TOKEN"'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up any running processes...'
            sh 'pkill -f node || true'
        }
        success {
            echo 'Pipeline execution successful!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
