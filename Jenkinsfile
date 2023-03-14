pipeline{
    agent any
    tools {
        maven "maven"
    }
    stages {
        
        stage('Git Checkout'){
            steps{
               git branch: 'main', url: 'https://github.com/tolux17tech/demo-counter-app.git' 
            }
        }

        stage("Unit Testing"){
            steps{
                sh 'mvn test'
            }
        }

        stage("Integration Testing"){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage("Build Artifact"){
            steps{
                sh 'mvn clean install'
            }
        }
        stage("Static Code analysis"){
            steps{ 
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                    sh "mvn clean package sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate Status"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }
            }
        }
        stage ("Upload file to nexus"){
            steps{
                script{
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'com.example', 
                        classifier: '', 
                        file: 'target/Uber.jar', 
                        type: 'jar']
                        ], 
                        credentialsId: 'Nexus2-login', 
                        groupId: 'com.example', 
                        nexusUrl: '172.21.0.4:8081', 
                        nexusVersion: 'nexus2', 
                        protocol: 'http', 
                        repository: 'counter-app-release', 
                        version: '1.0.0'
                }
            }
        }
        // stage ("Upload snapshot file to nexus"){
        //     steps{
        //         script{
        //             nexusArtifactUploader artifacts: [
        //                 [
        //                     artifactId: 'springboot', 
        //                 classifier: '', 
        //                 file: 'target/Uber.jar', 
        //                 type: 'jar']
        //                 ], 
        //                 credentialsId: 'Nexus2-login', 
        //                 groupId: 'com.example', 
        //                 nexusUrl: '172.21.0.4:8081', 
        //                 nexusVersion: 'nexus2', 
        //                 protocol: 'http', 
        //                 repository: 'counter-app-snapshot', 
        //                 version: '1.0.0'
        //         }
        //     }
        // }
    }
}