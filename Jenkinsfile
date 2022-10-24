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
        stage("Pushing to nexus"){
            steps{
                script{
                    sh "mvn deploy"
                }
            }
        }
        
    }
}
