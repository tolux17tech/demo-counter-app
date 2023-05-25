pipeline{
    agent any
    tools{
        maven maven
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
        }
    }