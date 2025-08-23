pipeline {
    agent any

    environment {
        DOTNET_HOME = "/usr/local/share/dotnet"
        TEST_RESULTS_UNIT = "SoftUniBazar.Tests/TestResults"
        TEST_RESULTS_INTEGRATION = "SoftUniBazar.IntegrationTests/TestResults"
        REPO_URL = "https://github.com/zolbooGnbt/Exam.git"
        BRANCH = "feature-ci-pipeline"
        WORK_DIR = "Exam"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository branch '${BRANCH}'..."
                sh "rm -rf ${WORK_DIR}" // clean existing folder
                sh "git clone -b ${BRANCH} ${REPO_URL} ${WORK_DIR}"
            }
        }

        stage('Restore') {
            steps {
                echo "Restoring NuGet packages..."
                sh "${DOTNET_HOME}/dotnet restore ${WORK_DIR}/SoftUniBazar.sln"
            }
        }

        stage('Build') {
            steps {
                echo "Building solution..."
                sh "${DOTNET_HOME}/dotnet build ${WORK_DIR}/SoftUniBazar.sln --configuration Release --no-restore"
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo "Running Unit Tests..."
                sh "mkdir -p ${WORK_DIR}/${TEST_RESULTS_UNIT}"
                sh "${DOTNET_HOME}/dotnet test ${WORK_DIR}/SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS_UNIT}/unit-tests.trx'"
            }
        }

        stage('Run Integration Tests') {
            steps {
                echo "Running Integration Tests..."
                sh "mkdir -p ${WORK_DIR}/${TEST_RESULTS_INTEGRATION}"
                sh "${DOTNET_HOME}/dotnet test ${WORK_DIR}/SoftUniBazar.IntegrationTests/SoftUniBazar.IntegrationTests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS_INTEGRATION}/integration-tests.trx'"
            }
        }

        stage('Publish Test Results') {
            steps {
                echo "Publishing test results..."
                junit "${WORK_DIR}/${TEST_RESULTS_UNIT}/*.trx"
                junit "${WORK_DIR}/${TEST_RESULTS_INTEGRATION}/*.trx"
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "✅ Build and tests succeeded!"
        }
        failure {
            echo "❌ Build or tests failed!"
        }
    }
}
