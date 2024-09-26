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
                script {
                    
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "KKDevOpsLabs-SNAPSHOT" : "KKDevOpsLabs-RELEASE"
                
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '7bbe7b2f-add6-41ba-a7ff-8ac739e8ff38', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.94:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"

                }

            }
        }

        // Stage4 : Print Environment Variables
        stage ('Print Environment Variables'){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
                echo "Group ID is '${GroupId}'"
            }
        }

          // Stage5 : Deploying
        stage ('Deploy to Tomcat'){
            steps {
                echo ' Deploying to Tomcat'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'AnsibleControlNode', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            excludes: '', 
                            execCommand: 'ansible-playbook -i /opt/playbooks/hosts /opt/playbooks/downloadanddeploy.yml', 
                            execTimeout: 120000, 
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                ])
                
            }
        }

          // Stage6 : Deploying the build artifact to Docker
        stage ('Deploy to Docker'){
            steps {
                echo ' Deploying to Docker'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'AnsibleControlNode', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            excludes: '', 
                            execCommand: 'ansible-playbook -i /opt/playbooks/hosts /opt/playbooks/downloadanddeploy_docker.yml', 
                            execTimeout: 120000, 
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                ])
                
            }

        }
    } 
}