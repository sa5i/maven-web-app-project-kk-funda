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
    }

        post {
        success {
            emailext (
                to: 'skdevops025@gmail.com',
                subject: "SUCCESS: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: """<p>Good news! The build <b>${env.BUILD_NUMBER}</b> of <b>${env.JOB_NAME}</b> completed successfully.</p>
                         <p>Check details at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html'
            )
        }
        failure {
            emailext (
                to: 'skdevops025@gmail.com',
                subject: "FAILURE: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: """<p>The build <b>${env.BUILD_NUMBER}</b> of <b>${env.JOB_NAME}</b> has failed.</p>
                         <p>Check details at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html'
            )
        }
        always {
            echo 'Build finished. Email notification attempted.'
        }
        
        
    }
}
