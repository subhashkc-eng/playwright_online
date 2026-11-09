pipeline {
    agent any

    tools {
        nodejs 'NodeJS-24'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Check Node and NPM') {
            steps {
                bat 'node --version'
                bat 'npm --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Install Playwright Browsers') {
            steps {
                bat 'npx playwright install chromium'
            }
        }

        stage('Execute Playwright Tests') {
            steps {
                bat 'npx playwright test'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'test-results/results.xml'

            archiveArtifacts artifacts: 'playwright-report/**',
                             allowEmptyArchive: true

            emailext(
                to: 'your-email@gmail.com',
                subject: "Playwright Build ${BUILD_NUMBER}: ${currentBuild.currentResult}",
                body: """
                Hello,

                Playwright execution has completed.

                Build Number: ${BUILD_NUMBER}
                Build Status: ${currentBuild.currentResult}

                Jenkins Build URL:
                ${BUILD_URL}

                Please check the Jenkins build for details.
                """
            )
        }
    }
}