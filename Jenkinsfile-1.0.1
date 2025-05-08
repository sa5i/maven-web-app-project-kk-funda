node
{

	echo "Git branch name is: ${env.BRANCH_NAME}"
	echo "Build number is: ${env.BUILD_NUMBER}" 
	echo "Build name is: ${env.JOB_NAME}"

	def mavenHome=tool name: "maven-3.9.9"

	

	stage('Git integration')
	{
		git branch: 'prod', credentialsId: 'github-pat', url: 'https://github.com/sa5i/maven-web-app-project-kk-funda.git'
	}

	stage('Compile')
	{
	sh "${mavenHome}/bin/mvn clean compile"
	}

	stage('Maven Build')
	{
	sh "${mavenHome}/bin/mvn clean package"
	}

	stage('SonarQ')
	{
	sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}
	 stage('Deploy to Nexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy to tomcat')
    {
         sh """
            curl -u admin:admin \
            --upload-file /var/lib/jenkins/workspace/stg-app-scripted-PL/target \
            "http://13.49.65.135:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }

}
