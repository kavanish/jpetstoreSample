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
	stage('Execute Maven') {
		steps {
		   script {
		
		curl https://artifactory.platformdxc-mg.com/artifactory/lma-maven/maven-org/mybatis/maven-jpetstore/maven-6.0.2-SNAPSHOT/ maven-jpetstore-maven-6.0.2-20200615.123226-2.war --output /var/lib/jenkins/workspace/ artifact-deployment-artifactory/target/maven-jpetstore-maven-6.0.2-SNAPSHOT.war
			}
		}
		
	}
	//stage('Example') {
          //  steps {
           //     echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
	//	
          //  }
        //}
}
}
