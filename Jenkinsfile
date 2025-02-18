pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ciro-Gallucci/rps-boot.git'
            }
        }

        stage('Build & Analyze') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Publish PMD Report') {
            steps {
                script {
                    // Pubblica il report PMD come HTML su Jenkins
                    publishHTML([target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/site',
                        reportFiles: 'pmd.html',
                        reportName: 'PMD Report'
                    ]])
                }
            }
        }

        stage('Record PMD Warnings') {
            steps {
                script {
                    recordIssues tools: [pmdParser(pattern: '**/target/pmd.xml')]
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/pmd.xml', fingerprint: true
        }
    }
}
