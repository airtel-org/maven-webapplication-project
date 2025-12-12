node{
try{
def maven=tool name:"maven"
stage('git checkout'){
notifyBuild('STARTED')
	git branch: 'dev', url: 'https://github.com/airtel-org/maven-webapplication-project.git'
}
stage('maven compile'){
	sh "${maven}/bin/mvn clean compile" 
}
stage('maven build'){
	sh "${maven}/bin/mvn clean package" 
}
stage('sonarqube'){
	sh "${maven}/bin/mvn clean package sonar:sonar" 
}
stage('nexus'){
	sh "${maven}/bin/mvn clean deploy" 
}
stage('tomcat deployment'){
	   sh """
	      curl -u nitheesh:password \
--upload-file /var/lib/jenkins/workspace/scriped-pipeline-1/target/maven-web-application.war \
"http://13.235.48.77:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """

}
}
 catch (e) {
   
       currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)       //function calling
  }


}


def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#278EF5'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#development')
  
}
