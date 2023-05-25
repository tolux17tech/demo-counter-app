pipeline{
    agent any
    tools{
        maven "maven"
    }
    stages{
        stage('Git Checkout from SCM'){
            steps{
                script{
                    git branch: 'dev', url: 'https://github.com/tolux17tech/demo-counter-app.git'
                }
            }
        }
    
        stage('Maven Unit Test'){
                steps{
                    script{
                    sh 'mvn test'
                    }
                }
            }
        stage('Maven Integration Test'){
            steps{
                script{
                sh 'mvn verify -DskipUnitTest'
                }
            }
          }
        stage('Maven build artifacts'){
            steps{
                script{
                    sh 'mvn clean install'
                }
            }
          }
        stage('Sonarqube Static Code Analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: "sonarqube_server_token"){
                        sh "mvn sonar:sonar"
                    }
                    
                }
            }
          }

        stage('Sonarqube Quality Gate Status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube_server_token'
                }
            }
          }

        stage('Nextus arifact uploader'){
            steps{
                script{
                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-info', groupId: 'com.example', nexusUrl: '44.204.44.171:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'demo-app-release', version: '1.0.0'
                }
            }
          }
        }
    }