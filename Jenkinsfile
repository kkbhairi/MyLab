pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish artifact to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'KKDevOpsLab', classifier: '', file: 'target/KKDevOpsLab-0.0.2-SNAPSHOT.war', type: 'war']], credentialsId: '3080e56e-97f6-4062-8d02-e63b20ac1dd7', groupId: 'com.kkdevopslab', nexusUrl: '172.20.10.97:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'KKDevOpsLabs-SNAPSHOT', version: '0.0.2-SNAPSHOT'

            }
        }

/*          // Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                } 

            }
        } */

        
        
    }

}