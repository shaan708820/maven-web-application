pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Static Code Analysis') {
            steps {
                // This now securely uses the 'sonar-token' credential you linked in Jenkins
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=maven-web-application'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // This strictly uses your new Ubuntu key to securely copy the file
                sshagent(['tomcat-key']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@51.20.105.18:/opt/tomcat/webapps/'    
                }
            }
        }
    }
}
