pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-jenkins-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url:'https://github.com/tanay25/flask-jenkins-cicd.git',
                credentialsId:772485eb-ea9d-44a5-83ab-d0f3d8a70ad8
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                docker run --rm $IMAGE_NAME pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop flask_app || true
                docker rm flask_app || true
                docker run -d -p 5000:5000 --name flask_app $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Flask CI/CD Pipeline Successful"
        }
        failure {
            echo "Pipeline Failed"
        }
    }
}
