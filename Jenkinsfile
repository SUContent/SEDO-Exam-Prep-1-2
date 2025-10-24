pipeline{
    agent any
    stages{
        stage("Restore the dependencies"){
            when {
              branch pattern: "main|develop|feature/.*", comparator: "REGEXP"
            }
            steps{
                bat 'dotnet restore'
            }
        }

        stage("Build the application"){
            when {
              branch pattern: "main|develop|feature/.*", comparator: "REGEXP"
            }
            steps{
                bat 'dotnet build --no-restore'
            }
        }

        stage("Run the tests"){
          when {
              branch pattern: "main|develop|feature/.*", comparator: "REGEXP"
            }
            steps{
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }

}