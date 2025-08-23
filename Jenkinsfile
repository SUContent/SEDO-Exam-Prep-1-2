pipeline {
    agent any

    environment {
        DOTNET_HOME = "/usr/local/share/dotnet"
        TEST_RESULTS_UNIT = "SoftUniBazar.Tests/TestResults"
        TEST_RESULTS_INTEGRATION = "SoftUniBazar.IntegrationTests/TestResults"
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
                sh "mkdir -p ${TEST_RESULTS_UNIT}"
                sh "mkdir -p ${TEST_RESULTS_INTEGRATION}"

                sh "${DOTNET_HOME}/dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS_UNIT}/unit-tests.trx'"

                sh "${DOTNET_HOME}/dotnet test SoftUniBazar.IntegrationTests/SoftUniBazar.IntegrationTests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS_INTEGRATION}/integration-tests.trx'"
            }
        }

        stage('Publish Test Results') {
            steps {

                junit "${TEST_RESULTS_UNIT}/*.trx"
                junit "${TEST_RESULTS_INTEGRATION}/*.trx"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
