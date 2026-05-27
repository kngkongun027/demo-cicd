pipeline {

    agent any

    stages {

        stage('Build Docker') {
            steps {
                bat 'docker build -t cicd-demo .'
            }
        }

        stage('Test') {
            steps {
                bat 'echo TEST SUCCESS'
            }
        }

        stage('Deploy Staging') {
            steps {

                bat 'docker stop staging || exit 0'
                bat 'docker rm staging || exit 0'

                bat '''
                docker run -d ^
                -p 3001:80 ^
                --name staging ^
                cicd-demo
                '''
            }
        }

        stage('Manual Approval') {
            steps {
                input 'Deploy to Production?'
            }
        }

        stage('Deploy Production') {
            steps {

                bat 'docker stop production || exit 0'
                bat 'docker rm production || exit 0'

                bat '''
                docker run -d ^
                -p 3000:80 ^
                --name production ^
                cicd-demo
                '''
            }
        }
    }
}