pipeline {
    agent any

    environment {
        DOTNET_PATH = "/usr/local/share/dotnet"
        PATH = "/usr/local/share/dotnet:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh "${DOTNET_PATH}/dotnet restore SoftUniBazar/SoftUniBazar.csproj"
            }
        }

        stage('Build') {
            steps {
                sh "${DOTNET_PATH}/dotnet build SoftUniBazar/SoftUniBazar.csproj --configuration Release --no-restore"
            }
        }

        stage('Test') {
            steps {
                sh '''
            mkdir -p SoftUniBazar.Tests/TestResults
            /usr/local/share/dotnet/dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj \
                --no-build \
                --verbosity normal \
                --logger "trx;LogFileName=TestResults/test-results.trx"
        '''
    }
}

    }

    post {
        always {
            // Publish test results in Jenkins
            junit 'TestResults/test-results.xml'
        }
    }
}
