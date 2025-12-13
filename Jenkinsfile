



pipeline {
    agent any

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_SKIP_FIRST_TIME_EXPERIENCE = '1'
    }

    stages {

        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            when {
                expression { fileExists('**/*Tests.csproj') }
            }
            steps {
                bat 'dotnet test --configuration Release --no-build'
            }
        }

        stage('Publish') {
            steps {
                bat 'dotnet publish -c Release -o publish_output'
            }
        }
    }

    post {
        success {
            echo 'Blazor application build completed successfully'
        }
        failure {
            echo 'Blazor application build failed'
        }
    }
}
