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
			 echo "jenkins publishes artifacts to jfrog"
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
						includePatterns: '**/target/*.jar',
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

                rtPublishBuildInfo(serverId: 'jenkins-jfrog-integ')
		    }
		}
	}
}
