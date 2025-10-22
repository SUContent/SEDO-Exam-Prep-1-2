pipeline
{
    agent any

    stages {
        stage('Restore Dependencies') {
            when { branch pattern: "(main|feature/.*)", comparator: "REGEXP" }
            steps {
                bat 'dotnet restore'
            }
        }
        stage('Build the application') {
            when { branch pattern: "(main|feature/.*)", comparator: "REGEXP" }
            steps {
                bat 'dotnet build --no-restore'
            }
        }
        stage('Run the Tests') {
            when { branch pattern: "(main|feature/.*)", comparator: "REGEXP" }
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
