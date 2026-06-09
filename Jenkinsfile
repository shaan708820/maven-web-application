pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pulls code from your GitHub repository
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Compiles the code and packages it into a .war file
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Static Code Analysis') {
            steps {
                // Injects the SonarQube environment and runs the analysis
                // Make sure your SonarQube Server is named 'SonarQubeServer' in Jenkins System settings
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=maven-web-application'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Uses the 'tomcat-key' credential to securely copy to your server
                sshagent(['tomcat-key']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@51.20.105.18:/opt/tomcat/webapps/'    
                }
            }
        }
    }
}
