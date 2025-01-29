pipeline {
    agent any

    stages {
        stage('checkout code') {
            steps {
                checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fazeel094/django-notes-app.git']])
            }
        }
        stage('build image') {
            steps {
                script{
                   sh "docker build -t b15me094/django-node ." 
                }
            }
        }
        stage('login to hub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhub')]) {
                    sh "docker login -u b15me094 -p ${dockerhub}"
}
                    sh "docker push b15me094/django-node:latest"
                }
            }
        }
        stage('deploy') {
            steps {
                script{
                    sh "docker rm -f django-node-container"
                    sh "docker run -d -p 8000:8000 --name django-node-container b15me094/django-node:latest"
                }
            }
        }
    }
}
