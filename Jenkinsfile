node {
    stage('Preparation') {
        catchError(buildResult: 'SUCCESS') {
            sh 'docker-compose down -v'
        }
    }
    stage('Checkout') {
        checkout scm
    }
    stage('Build and Deploy') {
        // Forceer DNS-instellingen voor docker-compose build
        sh 'DOCKER_BUILDKIT=1 docker-compose build --build-arg HTTP_PROXY --build-arg HTTPS_PROXY'
        sh 'docker-compose up -d'
    }
    stage('Test') {
        // Gebruik host-netwerk voor de test-container zodat DNS en NuGet direct werken
        sh 'docker build --network=host --target testrunner -t todoapp-tests .'
        sh 'docker run --rm --network=host todoapp-tests'
    }
}