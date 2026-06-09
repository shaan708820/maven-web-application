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
                sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@16.16.216.211:/opt/apache-tomcat-9.0.50/webapps/'    
                }
            }
        }
    }
}
