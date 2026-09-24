node {
    stage('Preparation') {
        catchError(buildResult: 'SUCCESS') {
            sh 'docker-compose down'
        }
    }
    stage('Checkout') {
        checkout scm
    }
    stage('Build and Deploy') {
        sh 'docker-compose up -d --build'
    }
    stage('Test') {
        sh 'ls -la'
    }
}