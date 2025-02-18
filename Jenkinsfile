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

        stage('Publish FindSecBugs Report') {
            steps {
                script {
                    // Pubblica il report FindSecBugs come HTML su Jenkins
                    publishHTML([target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target/findsecbugs',
                        reportFiles: 'findsecbugs.html',
                        reportName: 'FindSecBugs Report'
                    ]])
                }
            }
        }

        stage('Record FindSecBugs Warnings') {
            steps {
                script {
                    // Registra i warning di FindSecBugs
                    recordIssues(tools: [findSecBugs(pattern: '**/target/findsecbugs.xml')])
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/findsecbugs.xml', fingerprint: true
        }
    }
}
