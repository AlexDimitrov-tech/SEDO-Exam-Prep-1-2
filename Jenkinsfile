// Task 4 - Jenkins multibranch pipeline for Repo 2 (.NET app)
// Put this file in the ROOT of Repo 2, named exactly "Jenkinsfile".
// Builds the app and runs ALL tests (unit + integration) for main and feature/* branches.
// Works on a Windows Jenkins (bat) and on Linux/macOS (sh).

def run(String cmd) {
    if (isUnix()) { sh cmd } else { bat cmd }
}

pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            when { anyOf { branch 'main'; branch 'feature/*' } }
            steps {
                run 'dotnet restore'
            }
        }

        stage('Build') {
            when { anyOf { branch 'main'; branch 'feature/*' } }
            steps {
                run 'dotnet build --no-restore --configuration Release'
            }
        }

        stage('Test') {
            when { anyOf { branch 'main'; branch 'feature/*' } }
            steps {
                run 'dotnet test --no-build --configuration Release --verbosity normal'
            }
        }
    }

    post {
        success { echo "Build and all tests passed on ${env.BRANCH_NAME}" }
        failure { echo "Build or tests FAILED on ${env.BRANCH_NAME}" }
    }
}
