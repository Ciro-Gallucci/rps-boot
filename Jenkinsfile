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

        stage('Semgrep Analysis') {
            steps {
                sh '''
                pip install semgrep --quiet
                semgrep scan --config=auto --json > semgrep-results.json || true
                '''
            }
        }

        stage('Publish Semgrep Report') {
            steps {
                script {
                    publishHTML([target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: '.',
                        reportFiles: 'semgrep-results.json',
                        reportName: 'Semgrep Report'
                    ]])
                }
            }
            
            stage('Record Static Analysis Warnings') {
                steps {
                    script {
                        recordIssues tools: [
                            pmdParser(pattern: '**/target/pmd.xml'),
                            jsonAnalysis(pattern: 'semgrep-results.json')
                        ]
                    }
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
