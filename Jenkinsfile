pipeline {
    agent any

    tools {
        maven 'mvn3'
        jdk 'jdk17'
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
        SONARQUBE_ENV = 'SQ'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/master']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'github',
                        url: 'https://github.com/naren-30/onlinebookstore.git'
                    ]]
                )
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') { 
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "❌ Pipeline failed due to Quality Gate: ${qg.status}"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, SonarQube analysis, and Quality Gate passed!'
        }
        failure {
            echo '❌ Build, SonarQube analysis, or Quality Gate failed.'
        }
    }
}
