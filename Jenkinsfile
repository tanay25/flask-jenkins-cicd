pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-jenkins-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                url:'https://github.com/tanay25/flask-jenkins-cicd.git'
               
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

