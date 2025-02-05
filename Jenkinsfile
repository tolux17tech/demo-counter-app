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
                    def readPomVersion = readMavenPom file:'pom.xml'

                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "counter-app-snapshot": "counter-app-release"
                    
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: "${readPomVersion.artifactId}", 
                        classifier: '', 
                        file: 'target/Uber.jar', 
                        type: 'jar']
                        ], 
                        credentialsId: 'Nexus2-login', 
                        groupId: "${readPomVersion.groupId}", 
                        nexusUrl: '172.21.0.4:8081/nexus', 
                        nexusVersion: 'nexus2', 
                        protocol: 'http', 
                        repository: nexusRepo, 
                        version: "${readPomVersion.version}"
                }
            }
        }
        stage("Docker Image Build"){
            steps{
                script {
                    sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID tolux17tech/$JOB_NAME:latest"
                    
                }
            }
        }

        stage("Push Image to DockerHub"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker_token', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                    sh "docker push tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker tolux17tech/$JOB_NAME:latest"
                 }
              }
            }
        }
    }
}