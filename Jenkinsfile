pipeline {
    agent any

    environment {
        BUILD_DIR = "build"
        DEPLOY_DIR = "D:\\Nexturn\\ChinnakotlaJagannath-Nexturn-Programs\\M6_Devops_Assignments\\Exercise-4\\React-ToDo-App-CI-CD-Development"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        checkout scm
                    } catch (Exception e) {
                        error "Git checkout failed. Verify repository and branch settings."
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Lint Code') {
            steps {
                bat 'npx eslint . || exit /B 0'  // Ignores warnings
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
                bat 'serve -s build -l 3000 > NUL 2>&1 &'
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
            script {
                echo 'Cleaning up any running servers...'
                bat 'for /F "tokens=5" %a in (\'netstat -ano ^| find ":3000"\') do @taskkill /PID %a /F 2>NUL'
            }
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
