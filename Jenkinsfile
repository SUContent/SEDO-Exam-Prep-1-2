pipeline {
    agent any

    environment {
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
                sh '/usr/local/share/dotnet/dotnet restore SoftUniBazar/SoftUniBazar.csproj'
            }
        }

        stage('Build') {
            steps {
                sh '/usr/local/share/dotnet/dotnet build SoftUniBazar/SoftUniBazar.csproj --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    mkdir -p TestResults
                    /usr/local/share/dotnet/dotnet test SoftUniBazar/SoftUniBazar.csproj \
                        --no-build \
                        --verbosity normal \
                        --logger "junit;LogFileName=TestResults/test-results.xml"
                '''
            }
        }
    }

    post {
        always {
            junit 'TestResults/test-results.xml'
        }
    }
}
