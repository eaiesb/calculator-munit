#!/usr/bin/groovy
pipeline {
    environment {
        JAVA_HOME = tool('java')
    }
agent any
options {
disableConcurrentBuilds()
}
        stages {
		   stage ("calculator-munit-mule4"){
		      stages {
		          stage("Build calculator-munit-mule4_dev source code"){
				  steps {
                           slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
                           slackSend (color: "add8e6", message: 'Calculator-munit-mule4_dev Deployment Started')
                           buildsrc() 
		                   sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy'
                       }
				   }
		          stage ('Upload Files To Artifactory'){
				    steps {
					script {
					    sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
                           def server = Artifactory.server 'artifactory'
                           def uploadSpec = """{
                           "files": [
                           {
                           "pattern": "**/*.jar",
                           "target": "generic-local/Calculator-munit-mule4_$BUILD_NUMBER_dev/Calculator-munit-mule4.jar"
                           }
                           ]
                           }"""                 
                           def buildInfo1 = server.upload spec: uploadSpec
					}
					}
					
				  }
				  
		      }
		   
		   }

        }
}
