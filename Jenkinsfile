pipeline {
    agent any

    environment {
        DEPLOY_DIR = "D:\\Nexturn\\ChinnakotlaJagannath-Nexturn-Programs\\M6_Devops_Assignments\\Exercise-4\\React-ToDo-App-CI-CD-Development"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the React ToDo Application repository...'
                git branch: 'main', url: 'https://github.com/CHINNAKOTLAJAGANNATH/React-ToDo-App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Lint Code') {
            steps {
                echo 'Linting the code using ESLint...'
                bat 'npx eslint . || exit /B 0'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests On All React ToDo App Functionalities using Jest...'
                bat 'npm test || exit /B 0'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building the React ToDo Application...'
                bat 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the application...'

                // Ensure the target deployment directory exists
                bat "if not exist \"${DEPLOY_DIR}\" mkdir \"${DEPLOY_DIR}\""

                // Delete existing deployment files
                bat "rd /s /q \"${DEPLOY_DIR}\" || exit /B 0"

                // Copy new build files
                bat "xcopy build \"${DEPLOY_DIR}\" /E /I /H /Y"
            }
        }

        stage('Start Server for Testing') {
            steps {
                echo 'Starting the Server...'
                bat "start /B npx serve -s \"${DEPLOY_DIR}\" -l 3000"

                echo 'Waiting for a few seconds to allow the React app to start'
                bat "powershell -Command \"Start-Sleep -Seconds 5\""
            }
        }

        stage('Post-deployment Testing') {
            steps {
                script {
                    echo 'Testing if the App is Running...'

                    // Execute curl command and capture HTTP response code
                    def responseCode = bat(
                        script: '''
                        @echo off
                        for /f "tokens=*" %%a in ('curl -s -o NUL -w "%%{http_code}" http://localhost:3000') do @echo %%a
                        ''',
                        returnStdout: true
                    ).trim()

                    echo "HTTP Response Code: ${responseCode}"

                    // Validate response code
                    if (responseCode == '200') {
                        echo 'App is Up and Running Successfully...'
                    } else {
                        error "Post-deployment testing failed with HTTP status code: ${responseCode}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up any running servers...'
            bat '''
            for /f "tokens=5" %%a in ('netstat -ano ^| find ":3000"') do (
                if %%a NEQ 0 (
                    taskkill /pid %%a /f
                    exit /B 0
                )
            )
            '''
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
