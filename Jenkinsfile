pipeline {
    agent any

    environment {
        NODEJS_VERSION = '16'  // Adjust according to your project's requirements
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
                    def nodeHome = tool name: 'NodeJS', type: 'hudson.plugins.nodejs.tools.NodeJSInstallation'
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
                sh 'npm test'
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
                // Add your deployment command here, e.g., scp, rsync, or cloud provider CLI command
            }
        }
    }

    post {
        always {
            echo 'Cleaning up any running servers...'
        }
        success {
            echo 'Pipeline execution successful!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
