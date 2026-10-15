pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        DOCKER_EXE = 'C:\\Users\\ABDERRAHMANE\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        IMAGE = 'manguijr/tp4'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/abdoheddaya/tp4.git'
            }
        }

        stage('Building image') {
            steps {
                bat '"%DOCKER_EXE%" build -t %IMAGE%:%BUILD_NUMBER% .'
            }
        }

        stage('Test image') {
            steps {
                bat '"%DOCKER_EXE%" run --rm --entrypoint grep %IMAGE%:%BUILD_NUMBER% -q "Welcome BDCC" /usr/share/nginx/html/index.html'
            }
        }

        stage('Publish Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    bat 'powershell -NoProfile -Command "$env:DOCKER_TOKEN | & $env:DOCKER_EXE login -u $env:DOCKER_USER --password-stdin"'
                }
                bat '"%DOCKER_EXE%" push %IMAGE%:%BUILD_NUMBER%'
            }
        }
    }
}
