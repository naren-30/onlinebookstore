pipeline {
    agent any

    tools {
        maven 'mvn3'
        jdk 'jdk17'
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
               nexusArtifactUploader artifacts: [[artifactId: 'onlinebookstore', classifier: '', file: 'target/onlinebookstore.war', type: 'war']], credentialsId: 'nex', groupId: 'onlinebookstore', nexusUrl: '3.7.54.113:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '0.0.1-SNAPSHOT'
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
