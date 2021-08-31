node{
 
 def mavenHome = tool name: "maven3.8.2"
 
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
 
 
	stage('CheckOutCode')
	{ 
	git branch: 'devbranch', credentialsId: '106cdc12-c201-4eb2-ad4e-c1690f44ca1f', url: 'https://github.com/aayu-info-tech-ltd/maven-web-application.git'
	}
	
	stage('Build')
	{
	sh "${mavenHome}/bin/mvn clean package"
	}
	
	stage('SonarQubeReport')
	{
	sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}
	
	stage('UploadArtifactIntoNexus')
	{
	sh "${mavenHome}/bin/mvn clean deploy"
	}
	
	stage('DeployAppIntoTomcatServer')
	{ 
	sshagent(['70866c7e-c3d4-486b-aef9-92980e0a4d79']) 
	{
    sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.130.222.134:/opt/apache-tomcat-9.0.52/webapps"
	
    }
	}
	
	stage('SendEmailNotification')
	{ 
	emailext body: '''Build is Successfully 

Regards
S''', subject: 'Build success', to: 'jsareddy@gmail.com'
	
	}
}
