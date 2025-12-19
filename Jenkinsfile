pipeline{
	agent any
	tools{
	maven "maven"
	}
	stages{
	stage('git checkout'){
	steps{
	git branch: 'dev', url: 'https://github.com/airtel-org/maven-webapplication-project.git'
	}
	}
     stage('maven compile'){
     steps{
     sh "mvn clean compile"
     }
     }
     stage('maven build'){
     steps{
     sh "mvn package"
     }
     }
     stage('sonar'){
     steps{
     sh "mvn sonar:sonar"
     }
     }
     stage('nexus'){
     steps{
     sh "mvn deploy"
     }
     }
     stage('deploy'){
     steps{
     sh """
  curl -u kk:password \
--upload-file /var/lib/jenkins/workspace/declarative-prac/target/maven-web-application.war \
"http://13.204.75.126:8080/manager/text/deploy?path=/maven-web-application&update=true"
"""
     }
     }

	}
}


