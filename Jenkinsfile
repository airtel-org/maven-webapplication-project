node{
def maven=tool name:"maven"
stage('git checkout'){
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
"http://15.206.203.54:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """

}
}

