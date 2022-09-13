pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage("Compile") {
            steps {
                sh "mvn clean compile"
            }
        }
        stage("Unit Tests") {
            steps {
                sh "mvn test"
            }
        }
        stage("Integration Tests") {
            steps {
                sh "mvn verify"
            }
        }
        stage("Package") {
            steps {
                sh "mvn package -DskipTests=true"
            }
        }

        stage("upload artifacts") {
            steps {
			 
						  script {
			  
			  

			 def server = Artifactory.server "jenkins-jfrog-integ"
       def buildInfo = Artifactory.newBuildInfo()
       buildInfo.env.capture = true
       buildInfo.env.collect()
       def uploadSpec = """{
         "files": [
           {
             "pattern": "**/target/*.jar",
             "target": "/"
           }, {
             "pattern": "**/target/*.pom",
             "target": "/"
           }, {
             "pattern": "${WORKSPACE}/*.txt",
			 
			"target": "example-repo-local" 
           }
         ]
       }"""
       // Upload to Artifactory.
       server.upload spec: uploadSpec, buildInfo: buildInfo
       buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
       // Publish build info.
       server.publishBuildInfo buildInfo
	   }
		    }
		}
	}
}
