pipeline {
    agent any

    tools {
        // Jenkins Global Tool Configuration-д тохируулсан .NET SDK-ийг ашиглана
        dotnet 'dotnet-8'
    }

    environment {
        // Шаардлагатай орчны хувьсагч
        BUILD_CONFIGURATION = "Release"
    }

    stages {
        stage('Checkout') {
            steps {
                // Репо-г тодорхой branch-ээс татаж авах
                git branch: 'feature-ci-pipeline', url: 'https://github.com/<таны-username>/<repo-нэр>.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh "dotnet build --configuration ${BUILD_CONFIGURATION} --no-restore"
            }
        }

        stage('Unit Tests') {
            when {
                branch 'develop'
            }
            steps {
                sh "dotnet test --configuration ${BUILD_CONFIGURATION} --filter Category=Unit --no-build"
            }
        }

        stage('Integration Tests') {
            when {
                branch 'staging'
            }
            steps {
                sh "dotnet test --configuration ${BUILD_CONFIGURATION} --filter Category=Integration --no-build"
            }
        }
    }

    post {
        always {
            // Build болон тестийн лог файлуудыг хадгалах
            archiveArtifacts artifacts: '**/bin/**/*.dll', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline амжилттай дууслаа!'
        }
        failure {
            echo 'Pipeline амжилтгүй боллоо!'
        }
    }
}
