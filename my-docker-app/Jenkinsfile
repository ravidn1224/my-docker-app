pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = 'ravidocker285/my-docker-app'
    }

    triggers {
        githubPush() // 🔁 Trigger on GitHub push
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/my-docker-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | \
                    docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                '''
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}