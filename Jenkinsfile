pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sharlymonica-v/WebGoat/WebGoat.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('SAST - SonarQube') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('SCA - Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '''
                    --scan .
                    --format XML
                    --data "C:\\ProgramData\\Jenkins\\dependency-check"
                ''',
                odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
    }

    post {
        success {
            echo 'SAST and SCA completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
