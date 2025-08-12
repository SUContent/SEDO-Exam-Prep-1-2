pipeline {
    agent any
    stages {
      
        stage('ReBuild dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }
      
        stage('Rebuild the application') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }
      
        stage('Run all tests') {
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
