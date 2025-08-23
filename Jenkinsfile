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
        stage('Unit Tests') {
            steps {
                bat 'dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --filter "Category=Unit"'
            }
        }
        stage('Integration Tests') {
            steps {
                bat 'dotnet test SoftUniBazar.IntegrationTests/SoftUniBazar.IntegrationTests.csproj --filter "Category=Integration"'
            }
        }
    }
}
