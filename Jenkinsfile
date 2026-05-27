pipeline {

    agent any

    stages {

        stage('Build Docker') {
            steps {
                sh 'docker build -t cicd-demo .'
            }
        }

        stage('Test') {
            steps {
                echo 'TEST SUCCESS'
            }
        }

        stage('Deploy Staging') {
            steps {

                sh 'docker stop staging || true'
                sh 'docker rm staging || true'

                sh '''
                docker run -d \
                -p 3001:80 \
                --name staging \
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

                sh 'docker stop production || true'
                sh 'docker rm production || true'

                sh '''
                docker run -d \
                -p 3000:80 \
                --name production \
                cicd-demo
                '''
            }
        }
    }
}