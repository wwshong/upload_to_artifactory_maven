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
			 /*echo "jenkins publishes artifacts to jfrog"
			rtBuildInfo(captureEnv: true)

                rtMavenResolver (
                        id: 'resolver-unique-id',
                        serverId: 'jenkins-jfrog-integ',
                        releaseRepo: 'example-repo-local',
                        snapshotRepo: 'example-repo-local'
                )
                rtMavenDeployer(
                        id: 'deployer-unique-id',
                        serverId: 'jenkins-jfrog-integ',
						deployArtifacts: '**/target/*.jar',
                        releaseRepo: 'example-repo-local',
                        snapshotRepo: 'example-repo-local'
                )

                rtMavenRun(
                        tool: "maven 3.8.6", // using Maven configured under Jenkins Global Tools
                        pom: "pom.xml",
                        goals: "clean install -DskipTests=true",
                        resolverId: 'resolver-unique-id',
                        deployerId: 'deployer-unique-id'
                )

                rtPublishBuildInfo(serverId: 'jenkins-jfrog-integ')*/
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
