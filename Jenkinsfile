pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/MadeDiksaPitra/Hands-On-CI-CD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build('hello-world-app')
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    bat 'docker stop hello-world-app-test || exit 0'
                    bat 'docker rm hello-world-app-test || exit 0'
                    
                    bat 'docker stop hello-world-app-deploy || exit 0'
                    bat 'docker rm hello-world-app-deploy || exit 0'

                    bat 'docker run -d --name hello-world-app-test -p 3000:3000 hello-world-app'
                    bat 'docker exec hello-world-app-test npm install'
                    bat 'docker exec hello-world-app-test npm test'
                
                    bat 'docker stop hello-world-app-test'
                    bat 'docker rm hello-world-app-test'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    bat 'docker run -d -p 3000:3000 --name hello-world-app-deploy hello-world-app'
                }
            }
        }
    }
}