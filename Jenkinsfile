pipeline {
    agent any

    stages {
        stage('clone repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/trapkit/Maven']])
            }
        }
        stage('Build Maven') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t pratikkill/devops-integration .'
                }
            }
        }
        stage('Push Docker Image to Dockerhub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpasswd', variable: 'dockerhubpasswd')]) {
                    sh 'docker login -u pratikkill -p ${dockerhubpasswd}'
                    }
                    sh 'docker push pratikkill/devops-integration'
                }
            }
        }
    }
}