pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify .NET SDK version') {
            steps {
                bat 'dotnet --version'
            }
        }        
      
        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build App') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Run All Tests') {
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build succeeded.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
