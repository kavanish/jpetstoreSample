def server = Artifactory.server 'jenkins-artifactory-server'

		 //If artifactory is not defined in Jenkins, then create on:
		// def server = Artifactory.newServer url: 'Artifactory url', username: 'username', password: 'password'

//Create Artifactory Maven Build instance
def rtMaven = Artifactory.newMavenBuild()

//def buildInfo

pipeline {
    agent any

	tools {
		//jdk "Java-1.8"
		maven "Apache Maven 3.3.9"
	}

    stages {
        stage('Clone sources'){
            steps {
                git url: 'https://github.com/aknits081/jpetstore.git'
            }
        }


//	stage('Quality Gate') {
//		steps {
//			timeout(time: 1, unit: 'HOURS') {
			//Parameter indicates wether to set pipeline to UNSTABLE if Quality Gate fails
		        // true = set pipeline to UNSTABLE, false = don't
			// Requires SonarQube Scanner for Jenkins 2.7+
//			waitForQualityGate abortPipeline: false
//		       }
//		 }
//	}

	stage('Artifactory configuration') {
		
	   steps {
		script {
			rtMaven.tool = 'Apache Maven 3.3.9' //Maven tool name specified in Jenkins configuration
		
			rtMaven.deployer releaseRepo: 'lma-maven', snapshotRepo: 'lma-maven', server: server //Defining where the build artifacts should be deployed to
			
			//rtMaven.resolver releaseRepo:'lma-maven', snapshotRepo: 'lma-maven', server: server //Defining where Maven Build should download its dependencies from
			
			rtMaven.deployer.artifactDeploymentPatterns.addExclude("pom.xml") //Exclude artifacts from being deployed
			
			//rtMaven.deployer.deployArtifacts =false // Disable artifacts deployment during Maven run
		    
			buildInfo = Artifactory.newBuildInfo() //Publishing build-Info to artifactory
			
			buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true

			buildInfo.env.capture = true
			}
	    }
	}

	stage('Execute Maven') {
		steps {
		   script {
		
		rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
			}
		}
		
	}

//	stage('Publish build info') {
//		steps {
//		  script {
//
//		server.publishBuildInfo buildInfo
//		}
//		}
//	}
	    
         stage('Artifactory download'){
            steps {
                script{
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
                    //def server = Artifactory.server SERVER_ID

                    // Read the download and upload specs:
                    def downloadSpec = readFile 'props-download.json'
                    //def uploadSpec = readFile 'jenkins-examples/pipeline-examples/resources/props-upload.json'

                    // Download files from Artifactory:
                    def buildInfo1 = server.download spec: downloadSpec
                    // Upload files to Artifactory:
                    //def buildInfo2 = server.upload spec: uploadSpec

                    // Merge the local download and upload build-info instances:
                    //buildInfo1.append buildInfo2

                    // Publish the merged build-info to Artifactory
                    //server.publishBuildInfo buildInfo1
                }
            }
        }
}
}
