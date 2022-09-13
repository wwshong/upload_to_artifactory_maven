pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage("create artifacts") {
            steps {
     
                 sh 'echo jenkins publishes artifacts to jfrog >> jenkins-jfog.txt '
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
                        releaseRepo: 'example-repo-local',
                        snapshotRepo: 'example-repo-local'
                )

                rtMavenRun(
                        tool: "apache-maven-3.6.3", // using Maven configured under Jenkins Global Tools
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