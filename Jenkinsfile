pipeline {
    agent any

    environment {
        PROJECT_DIR = "C:\\Users\\Dell-Lap\\Downloads\\react-hello-world-app-master\\react-hello-world-app-master"
        TOMCAT_DIR = "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps"
        APP_NAME = "hello-world-app" // The default app name
        DEPLOY_DIR = "C:\\Users\\Dell-Lap\\Downloads\\node" // Specify the folder you want to deploy to
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git credentialsId: 'muthu512', url: 'https://github.com/muthu512/tomcat.git', branch: 'master'
            }
        }

        stage('Check Node and npm Versions') {
            steps {
                script {
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

        stage('Deploy to Custom Folder') {
            steps {
                script {
                    // Check if the build directory exists and contains files
                    bat "if not exist \"${PROJECT_DIR}\\build\" (echo Build directory does not exist && exit 1)"
                    bat "dir \"${PROJECT_DIR}\\build\""
                    bat "if not exist \"${PROJECT_DIR}\\build\\*\" (echo No files in build directory && exit 1)"

                    // Create the deployment directory if it does not exist
                    bat "if not exist \"${DEPLOY_DIR}\" mkdir \"${DEPLOY_DIR}\""

                    // Copy files from build directory to the specified custom directory
                    bat "xcopy /S /I /Y \"${PROJECT_DIR}\\build\\*\" \"${DEPLOY_DIR}\\\""
      
