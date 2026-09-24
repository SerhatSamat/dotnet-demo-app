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
        // Dit zoekt automatisch naar een .sln of .csproj in de rootmap
        sh 'docker run --rm -v $(pwd):/app -w /app mcr.microsoft.com/dotnet/sdk:10.0 dotnet test'
    }
}