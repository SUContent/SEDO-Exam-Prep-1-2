pipeline{
    agent any
    stages{
        stage("Restore dependencies"){
            when{
                anyOf{
                    branch 'main'
                    branch 'feature'
                }
            }
            steps{
                bat 'dotnet restore'
            }
        }
        stage("Build"){
            when{
                anyOf{
                   branch 'main'
                    branch 'feature'
                }
            }
            steps{
                bat 'dotnet build --no-restore'
            }
        }
        stage("Run tests"){
            when{
                anyOf{
                   branch 'main'
                  branch 'feature'
                }
            }
            steps{
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}