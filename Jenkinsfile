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
                // Now using the dedicated Tomcat key
                sshagent(['tomcat-key']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@16.16.216.211:/opt/apache-tomcat-9.0.50/webapps/'    
                }
            }
        }
    }
}
