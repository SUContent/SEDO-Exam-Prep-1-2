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
                        --logger "trx;LogFileName=TestResults/test-results.trx"
                '''
            }
        }
    }

    post {
        always {
            // Convert TRX to JUnit format and publish results
            // You can use dotnet tool "trx2junit" or similar, or just use the JUnit plugin if you generate JUnit XML directly
            // Example assuming conversion is done and XML is in TestResults/test-results.xml:
            junit 'TestResults/*.xml'
        }
    }
}
