pipeline {
    agent any
    stages {
        stage('Restore') {
            steps {
                sh 'dotnet restore HouseRentingSystem.sln'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build HouseRentingSystem.sln --configuration Release'
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test HouseRentingSystem.Tests/HouseRentingSystem.Tests.csproj'
            }
        }
    }
}