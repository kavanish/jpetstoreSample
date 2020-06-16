pipeline {
    agent any

    stages {
        stage ('Compile Stage') {
            steps {
                withMaven(maven : 'LocalMaven') {
                    sh 'mvn clean install'
                }
            }
        }
    stage ('Testing Stage') {
            steps {
                withMaven(maven : 'LocalMaven') {
                    sh 'mvn test'
                }
            }
        }
        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'LocalMaven') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}
