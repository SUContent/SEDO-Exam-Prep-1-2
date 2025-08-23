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
                sh """
                    mkdir -p TestResults
                    ${DOTNET_PATH}/dotnet test SoftUniBazar/SoftUniBazar.csproj \\
                        --no-build \\
                        --verbosity normal \\
                        --logger "junit;LogFileName=TestResults/test-results.xml"
                """
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
