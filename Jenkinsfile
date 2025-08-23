// pipeline {
//     agent any

//     environment {
//         DOTNET_HOME = "/usr/local/share/dotnet"
//         TEST_RESULTS = "SoftUniBazar.Tests/TestResults"
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Restore') {
//             steps {
//                 sh "${DOTNET_HOME}/dotnet restore SoftUniBazar.sln"
//             }
//         }

//         stage('Build') {
//             steps {
//                 sh "${DOTNET_HOME}/dotnet build SoftUniBazar.sln --configuration Release --no-restore"
//             }
//         }

//         stage('Test') {
//             steps {
//                 sh "mkdir -p ${TEST_RESULTS}"
                
//                 sh "${DOTNET_HOME}/dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --no-build --logger 'trx;LogFileName=${TEST_RESULTS}/test-results.trx'"

//             }
//         }

//         stage('Publish Test Results') {
//             steps {
//                 junit "${TEST_RESULTS}/*.trx"
//             }
//         }
//     }

//     post {
//         always {
//             cleanWs()
//         }
//     }
// }
pipeline {
    agent any

    environment {
        DOTNET_SKIP_FIRST_TIME_EXPERIENCE = 'true'
        DOTNET_CLI_TELEMETRY_OPTOUT = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                script {
                    sh 'dotnet restore SoftUniBazar.sln'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'dotnet build SoftUniBazar.sln --configuration Release --no-restore'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests and output NUnit results
                    sh 'dotnet test SoftUniBazar.Tests/SoftUniBazar.Tests.csproj --logger "trx;LogFileName=tests.trx" --no-build'
                    sh 'dotnet test SoftUniBazar.IntegrationTests/SoftUniBazar.IntegrationTests.csproj --logger "trx;LogFileName=integrationtests.trx" --no-build'
                }

                // Publish test results so Jenkins can show green/red icons
                junit '**/TestResults/*.trx'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/bin/Release/**', allowEmptyArchive: true
        }

        success {
            echo 'Build succeeded! ✅'
        }

        unstable {
            echo 'Build succeeded with warnings. ⚠️'
        }

        failure {
            echo 'Build failed! ❌'
        }
    }
}
