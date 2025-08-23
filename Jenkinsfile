pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Restore') {
            steps {
                sh 'dotnet restore SoftUniBazar/SoftUniBazar.csproj'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build SoftUniBazar/SoftUniBazar.csproj --configuration Release --no-restore'
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
    post {
        always {
            junit '**/TestResults/*.xml'
        }
    }
}
