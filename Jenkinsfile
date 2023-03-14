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
    }
}