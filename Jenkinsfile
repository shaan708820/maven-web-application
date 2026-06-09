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

        stage('Deploy to Tomcat') {
            steps {
                // Updated to match the exact ID from your screenshot
                sshagent(['ansible-ssh-key-credential']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@16.16.216.211:/opt/apache-tomcat-9.0.50/webapps/'    
                }
            }
        }
    }
}
