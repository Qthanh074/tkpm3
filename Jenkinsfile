pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git branch: 'main', url: 'https://github.com/huudqtmu/projectnet.git'
            }
        }

        stage('Restore package') {
            steps {
                echo 'Restore package'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Build project netcore'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Tests') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish to folder') {
            steps {
                echo 'Publishing...'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Publish to IIS folder') {
            steps {
                echo 'Copying to IIS wwwroot'
                bat 'xcopy "%WORKSPACE%\\publish" /E /Y /I /R "C:\\inetpub\\wwwroot\\tkpm3"'
            }
        }

        stage('Deploy to IIS') {
            steps {
                powershell '''
                Import-Module WebAdministration
                if (-not (Test-Path IIS:\\Sites\\tkpm3)) {
                    New-Website -Name "tkpm3" -Port 81 -PhysicalPath "C:\\inetpub\\wwwroot\\tkpm3"
                }
                '''
            }
        }
    }
}
