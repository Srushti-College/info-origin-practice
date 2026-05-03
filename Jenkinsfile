pipeline {
    agent any

    environment {
        IMAGE_NAME = "srushti22/flask-app"
        TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Srushti-College/info-origin-practice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t srushti22/flask-app:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push  srushti22/flask-app:latest'
                }
            }
        }

        stage('Update K8s Manifest') {
            steps {
                sh '''
                sed -i "s|image: .*|image:  srushti22/flask-app:latest|g" deployment.yaml
                '''
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
