pipeline {
    agent any

    environment {
        DOTNET_HOME = "/usr/local/share/dotnet"
        TEST_RESULTS = "SoftUniBazar.Tests/TestResults"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh "${DOTNET_HOME}/dotnet restore SoftUniBazar.sln"
            }
        }

        stage('Build') {
            steps {
                sh "${DOTNET_HOME}/dotnet build SoftUniBazar.sln --configuration Release --no-restore"
            }
        }

        stage('Test') {
            steps {
                sh "mkdir -p ${TEST_RESULTS}"
                
                sh "${DOTNET_HOME}/dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS}/test-results.trx'"

            }
        }

        stage('Publish Test Results') {
            steps {
                junit "${TEST_RESULTS}/*.trx"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
