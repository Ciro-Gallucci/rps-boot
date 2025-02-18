pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ciro-Gallucci/rps-boot.git'
            }
        }

        stage('Build & Static Analysis') {
            steps {
                // Esegui Maven con il comando che include FindSecBugs
                script {
                    // Esegui il comando Maven con FindSecBugs
                    sh 'mvn clean install findsecbugs:check'
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
