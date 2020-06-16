pipeline {
    agent any

    stages {
        tools {
            jdk "java-1.8.0"
            maven "Maven 3.6.0"
        }
    stages {
        stage ('Execute Maven') {
            script {
              rtMaven.run pom: 'pom.xml', goals: 'clean install'  
            }
        }
    }
}
