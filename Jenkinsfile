pipeline {
    agent any
    tools { nodejs: 'node' }

    environment {
        APP  = "${env.BRANCH_NAME = 'main' ? 'nodemain' : 'nodedev'}"
        TAG  = "v1.0"
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'npm install' }
        }
        stage('Test') {
            steps { sh 'CI=true npm test -- --watchAll=false' }
        }
        stage('Build docker image') {
            steps { sh "docker build -t ${APP}:${TAG} ." }
        }
        stage('Deploy') {
            steps {
                sh "docker rm -f ${APP} || true"
                sh "docker run -d --name ${APP} -p ${PORT}:3000 ${APP}:${TAG}"
            }
        }
    }
}