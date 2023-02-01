pipeline {
    environment {
        registry = 'liowino/docker-jenkins'
        registryCredentials = 'DOCKER-HUB-CREDENTIALS'
    }
    agent any
    stages {
        stage('Cloning our gihub repository') {
            steps {
                git credentialsId:'GIT-ACCESS-TOKEN', url: 'https://github.com/liowino/app-py.git', branch: 'main'
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t app-py-image .'
            }
        }
        stage('Tag docker image') {
            steps {
                sh 'docker tag app-py-image:latest liowino/docker-jenkins:$BUILD_NUMBER'
                sh 'docker tag app-py-image:latest liowino/docker-jenkins:latest'
            }
        }
        stage('Push image to repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER-HUB-CREDENTIALS', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                   sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                   sh 'docker push $registry:$BUILD_NUMBER'
                   sh 'docker push $registry:latest'
                }
            }
        }
    }
}
    
