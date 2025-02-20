pipeline {
    agent any
    tools {
        maven 'mvn' 
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ciro-Gallucci/rps-boot.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Static Analysis - FindSecurityBugs') {
            steps {
                sh 'mvn spotbugs:spotbugs'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/spotbugsXml.xml', fingerprint: true
                }
            }
        }
    }
}
