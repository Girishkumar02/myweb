try{
	node{
	    properties([parameters([choice(choices: ['master', 'dev', 'qa', 'staging'], description: 'Choose branch to build and deploy', name: 'gitBranch')]), pipelineTriggers([pollSCM('')])])
    stage('Git Checkout'){
         git credentialsId: 'd99cba4b-0929-42b3-b3e4-8f6ee11b486b', url: 'https://github.com/Girishkumar02/myweb.git'
    }
	
	stage('Maven Build'){
		sh 'mvn clean package'
	}
	stage('Deploy to Dev'){
		sh 'mv target/*.war target/myweb.war'
		sshagent(['tomcat-dev']) {
			sh 'ssh ec2-user@13.233.216.44 rm -rf /opt/tomcat9.0.74/webapps/myweb*'
		    sh 'scp target/myweb.war ec2-user@13.233.216.44:/opt/tomcat9.0.74/webapps/'
		    sh 'ssh ec2-user@13.233.216.44 sudo service tomcat restart'
		}
	    slackSend channel: '#devops-2',
				  color: 'good',
				  message: "Job -  ${env.JOB_NAME}, Completed successfully Build URL is ${env.BUILD_URL}"


	}
}

}catch(error){
  slackSend channel: '#devops-2',
				  color: 'danger',
				  message: "Job -  ${env.JOB_NAME}, Failed, Build URL is ${env.BUILD_URL}"
   error 'Something wrong'
}
