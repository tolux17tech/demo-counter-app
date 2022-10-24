pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage("Tesing with mvn"){
            steps{
                script{
                    sh "mvn test"
                }
            }
        }
        stage("Building war"){
            steps{
                script{
                    sh "mvn clean package"
                }
            }
        }
        stage("Testing with sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqubeconfigured') {
                    sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage ("Sonarqube quality gate status"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubeconfigured'
                }
            }
        }
        stage("Upload jar to nexus"){
            steps{
                script{
                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "tesla-land-snapshot" : "tesla-land-release"
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', file: 'target/Uber.jar', 
                            type: 'jar']
                            ], 
                            credentialsId: 'e161dcd8-ce01-45f9-b023-efc0ea5b6a58', 
                            groupId: 'com.example', 
                            nexusUrl: '10.0.0.168:4000', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: nexusRepo, 
                            version: "${readPomVersion.version}"
                }
            }
        }

        stage("docker build"){
            steps{
                script{
                    sh "docker build . -t $JOB_NAME:v1.$BUILD_ID"
                    sh "docker tag $JOB_NAME:v1.$BUILD_ID tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                }
            }
        }
        stage("docker push") {
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS |docker login -u $USER --password-stdin"
                    sh "docker push tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                   }
                }
            }
        }
        stage("docker run"){
            steps{
                script{
                    echo "coming soon"
                   
                    sh "docker run -d -p9090:9099 tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                }
            }
        }
        
    }
}
