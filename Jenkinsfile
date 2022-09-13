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
			  
			  
                rtBuildInfo(captureEnv: true)

                
                rtMavenDeployer(
                        id: 'deployer-unique-id',
                        serverId: 'jenkins-jfrog-integ',
                        releaseRepo: 'libs-release',
                        snapshotRepo: 'libs-snapshot',
                        includePatterns: ["*.jar"]
                )

                rtMavenRun(
                        tool: "maven 3.8.6", // using Maven configured under Jenkins Global Tools
                        pom: "pom.xml",
                        goals: "clean install package -DskipTests=true",
                        
                        deployerId: 'deployer-unique-id'
                )

                rtPublishBuildInfo(serverId: 'jenkins-jfrog-integ')
	   }
		    }
		}
	}
}
