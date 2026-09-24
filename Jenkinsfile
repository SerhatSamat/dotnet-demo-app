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
        sh 'docker run --rm -v $(pwd):/app -w /app/TodoApp.Tests mcr.microsoft.com/dotnet/sdk:10.0 dotnet test'
    }
}