pipeline {
    agent any

    environment {
        BUILD_DIR = "build"
        DEPLOY_DIR = "D:\\Nexturn\\ChinnakotlaJagannath-Nexturn-Programs\\M6_Devops_Assignments\\Exercise-4\\React-ToDo-App-CI-CD-Development"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/CHINNAKOTLAJAGANNATH/React-ToDo-App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Lint Code') {
            steps {
                bat 'npx eslint . || exit /B 0'  // Prevent pipeline failure due to warnings
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Application') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    if (!fileExists("${BUILD_DIR}")) {
                        error "Build folder not found. Build might have failed."
                    }
                }
                bat "if not exist \"${DEPLOY_DIR}\" mkdir \"${DEPLOY_DIR}\""
                bat "xcopy /E /I /H /Y ${BUILD_DIR} \"${DEPLOY_DIR}\""
            }
        }

        stage('Start Server for Testing') {
            steps {
                bat 'serve -s build -l 3000 &'
            }
        }

        stage('Post-deployment Testing') {
            steps {
                bat 'curl -Is http://localhost:3000 | find "200 OK"'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up any running servers...'
            bat 'for /F "tokens=5" %a in (\'netstat -ano ^| find ":3000"\') do (taskkill /PID %a /F) || exit /B 0'
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
