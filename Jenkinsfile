pipeline {
    agent any
    tools {
        maven 'maven-3.9.9'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'prod', credentialsId: 'github-pat', url: 'https://github.com/sa5i/maven-web-app-project-kk-funda.git'
            }
        }
        stage('Maven compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                sh 'mvn clean sonar:sonar'
            }
        }
        stage('Nexus Upload') {
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sh """
            curl -u admin:admin \
            --upload-file /var/lib/jenkins/workspace/stg-app-declarative-PL/target \
            "http://13.49.65.135:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
            }
        }

        post {
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The Jenkins job succeeded!\n\nCheck details: ${env.BUILD_URL}",
                to: "skdevops025@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The Jenkins job failed.\n\nCheck details: ${env.BUILD_URL}",
                to: "skdevops025@gmail.com"
            )
        }
        always {
            echo 'Build finished. Notification sent.'
        }
        
        
    }
}
