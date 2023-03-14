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
                withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                    sh "mvn sonar:sonar"
               }
            }
        }
    }
}