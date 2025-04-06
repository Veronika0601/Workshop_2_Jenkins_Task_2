pipeline {
    agent any

    stages {
        stage("Checkout code") {
            steps {
                git branch: 'main', url: 'https://github.com/Veronika0601/Workshop_2_Jenkins_Task_2'
            }
        }

        stage("Set up dot net 6") {
            steps {
                bat '''
                    curl --ssl-no-revoke -L -o dotnet-sdk-6.0.136-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/f7328260-56c1-48b5-a2ec-6b8e84548da1/624e4c12d20f7e56c3e0e8e2821f2b89/dotnet-sdk-6.0.136-win-x64.exe
                    dotnet-sdk-6.0.136-win-x64.exe /quiet /norestart
                    dotnet --info
                '''
            }
        }

        stage("Install nuget packages") {
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }

        stage("Build the project") {
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln'
            }
        }

        stage("Run tests for TestProject1") {
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage("Run tests for TestProject2") {
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            // Ако ползваш MSTest плъгин:
            // step([
            //     $class: 'MSTestPublisher',
            //     testResultsFile: '**/TestResults/*.trx'
            // ])
        }
    }
}
