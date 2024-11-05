pipeline {
    agent any
}

environment {
    PROJECT_DIR = "C:\\Users\\Dell-Lap\\Downloads\\react-hello-world-app-master\\react-hello-world-app-master"
    TOMCAT_DIR = "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps"
    APP_NAME = "hello-world-app"
}

stages {
    stage('Checkout SCM') {
        steps {
            git credentialsId: 'muthu512', url: 'https://github.com/muthu512/TC-SERVER', branch: 'master'
        }
    }

    stage('Check Node and npm Versions') {
        steps {
            script {
                // Check if Node.js and npm are installed
                bat 'node -v || exit 1'
                bat 'npm -v || exit 1'
            }
        }
    }

    stage('Check Environment Variables') {
        steps {
            script {
                bat 'echo %PATH%'
            }
        }
    }

    stage('Prepare Project') {
        steps {
            script {
                dir(PROJECT_DIR) {
                    bat 'if exist "package.json" (echo package.json exists) else (echo package.json not found && exit 1)'
                }
            }
        }
    }

    stage('Install Dependencies') {
        steps {
            script {
                dir(PROJECT_DIR) {
                    bat 'npm install'
                }
            }
        }
    }

    stage('Optimized Build React App') {
        steps {
            script {
                dir(PROJECT_DIR) {
                    try {
                        bat 'npm run build || exit 1'
                    } catch (Exception e) {
                        echo "Build failed: ${e.message}"
                        currentBuild.result = 'FAILED'
                    }
                }
            }
        }
    }

    stage('Deploy to Tomcat') {
        steps {
            script {
                // Check if the build folder exists and contains files
                bat "if not exist \"${PROJECT_DIR}\\build\" (echo Build directory does not exist && exit 1)"
                bat "dir \"${PROJECT_DIR}\\build\""

                // If the build directory exists but is empty, display an error and exit
                bat "if not exist \"${PROJECT_DIR}\\build\\*\" (echo No files in build directory && exit 1)"

                // Create the application directory if it does not exist
                bat "if not exist \"${TOMCAT_DIR}\\${APP_NAME}\" mkdir \"${TOMCAT_DIR}\\${APP_NAME}\""

                // Copy files from build directory to Tomcat
                bat "xcopy /S /I /Y \"${PROJECT_DIR}\\build\\*\" \"${TOMCAT_DIR}\\${APP_NAME}\\\""

                // Verify deployment by listing files in the Tomcat app directory
                bat "dir \"${TOMCAT_DIR}\\${APP_NAME}\""
            }
        }
    }
}

post {
    always {
        echo 'This will always run after the pipeline finishes'
    }
    success {
        echo 'Pipeline succeeded!'
    }
    failure {
        echo 'Pipeline failed!'
    }
    unstable {
        echo 'Pipeline unstable!'
    }
}