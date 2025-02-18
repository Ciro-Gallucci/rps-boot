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
                    // Raccogli i warning di PMD
                    recordIssues tools: [pmd(pattern: '**/target/pmd.xml')]
                }
            }
        }

        stage('Security Analysis with FindSecBugs') {
            steps {
                sh 'mvn spotbugs:check'
            }
        }
        
        stage('Publish FindSecBugs Report') {
            steps {
                script {
                    publishHTML([target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target',
                        reportFiles: 'spotbugsXml.xml',
                        reportName: 'FindSecBugs Report'
                    ]])
                }
            }
        }
        
        stage('Record Warnings') {
            steps {
                script {
                    // Raccogli i warning di PMD
                    recordIssues tools: [pmd(pattern: '**/target/pmd.xml')]
        
                    // Raccogli i warning di FindSecBugs
                    recordIssues tools: [spotBugs(pattern: '**/target/spotbugsXml.xml')]
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/pmd.xml, target/spotbugsXml.xml', fingerprint: true
        }
    }
}
