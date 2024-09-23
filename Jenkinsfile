pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()

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
                nexusArtifactUploader artifacts: 
                [[artifactId: 'KKDevOpsLab', 
                classifier: '', 
                file: 'target/KKDevOpsLab-0.0.2-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: '7bbe7b2f-add6-41ba-a7ff-8ac739e8ff38', 
                groupId: 'com.kkdevopslab', 
                nexusUrl: '172.20.10.94:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'KKDevOpsLabs-SNAPSHOT', 
                version: '0.0.2-SNAPSHOT'

            }
        }

        // Stage4 : Print Environment Variables
        stage ('Print Environment Variables'){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name os '${Name}'"
                echo "Group ID is '${GroupId}'"
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