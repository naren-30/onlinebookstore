pipeline {
    agent any

    tools {
        maven 'mvn3'       // Make sure you configured Maven in Jenkins Global Tool Config
        jdk 'jdk17'        // Or whichever JDK your project needs
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/naren-30/onlinebookstore.git']])
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment to Nexus successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
