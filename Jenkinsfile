pipeline {
    agent any

    environment {
        IMAGE_NAME = "srushti22/python-app"
        TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Srushti-College/info-origin-practice.git
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t  python-app :latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push  python-app :latest'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop python-app || true
                docker rm python-app || true
                docker run -d -p 5000:5000 --name python-app  python-app :latest
                '''
            }
        }
    }
}
