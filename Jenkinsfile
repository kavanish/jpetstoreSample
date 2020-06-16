pipeline {
    agent any
    
    stages {
        stage {
            tools {
                jdk "java-1.8.0"
                maven "Maven 3.6.0"
            }
        }
        stage ('Execute Maven') {
            script {
                rtMaven.run pom: 'pom.xml', goals: 'clean install'  
            } 
        }
    
    }
}
