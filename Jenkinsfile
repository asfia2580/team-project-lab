pipeline {
    agent any

    environment {
        DOCKER_BIN = 'C:\\Users\\PC\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        REGISTRY   = 'localhost:5000'
        IMAGE_NAME = 'html-app'
        PORT       = '8081'
    }

    stages {
        stage('Checkout & Verify') {
            steps {
                bat 'dir'
                bat 'if not exist index.html (echo index.html is missing & exit /b 1)'
                bat 'if not exist Dockerfile (echo Dockerfile is missing & exit /b 1)'
            }
        }

        stage('Build Image') {
            steps {
                bat '"%DOCKER_BIN%" build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .'
            }
        }

        stage('Push to Local Registry') {
            steps {
                bat '"%DOCKER_BIN%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'
                bat '"%DOCKER_BIN%" push %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '"%DOCKER_BIN%" rm -f %IMAGE_NAME% >nul 2>&1'
                bat '"%DOCKER_BIN%" run -d --name %IMAGE_NAME% -p %PORT%:80 %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }

        stage('Health Check') {
            steps {
                bat 'ping -n 10 127.0.0.1 >nul'
                bat 'curl -f http://localhost:%PORT%'
            }
        }
    }
}
