pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                bat 'dotnet restore SoftUniBazar.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build SoftUniBazar.sln --configuration Release'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --configuration Release'
            }
        }
    }

    post {
        always {
            echo '🧹 Pipeline finished.'
        }
        success {
            echo '✅ All stages completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}