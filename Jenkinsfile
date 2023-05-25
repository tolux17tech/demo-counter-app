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
        // stage('Sonarqube Static Code Analysis'){
        //     steps{
        //         script{
        //             withSonarQubeEnv(credentials: "sonarqube_server_token")
        //             sh "mvn sonar:sonar"
        //         }
        //     }
        //   }
        }
    }