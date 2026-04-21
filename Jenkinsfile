pipeline {
    agent any

    environment {
        IMAGE_NAME = "sourav1304021/django-argocd"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/souravsingha13/django-argocd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Update K8s YAML') {
            steps {
                sh '''
                sed -i 's|image:.*|image: sourav1304021/django-argocd:latest|' k8s/deployment.yaml
                '''
            }
        }

        stage('Push to GitHub') {
            steps {
                sh '''
                git config user.email "jenkins@example.com"
                git config user.name "jenkins"
                git add .
                git commit -m "Update image"
                git push
                '''
            }
        }
    }
}