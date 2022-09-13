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

                //enabling rtMavenResolver doesn’t work
				//the SOURCE
				//where the Maven build should download its dependencies from
				/*rtMavenResolver (
                        id: 'resolver-unique-id',
                        serverId: 'jenkins-jfrog-integ',
                        releaseRepo: 'all',
                        snapshotRepo: 'all'
                )*/
				
				//the DESTINATION
				//define where our build artifacts should be deployed to. we define the Artifactory server and repositories on the 'rtMaven' instance
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
                        //resolverId: 'resolver-unique-id',
                        deployerId: 'deployer-unique-id'
                )

                rtPublishBuildInfo(serverId: 'jenkins-jfrog-integ')
	   }
		    }
		}
	}
}
