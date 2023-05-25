pipeline{
    agent any
    stages{
        stage('Git Checkout from SCM'){
            steps{
                script{
                    git branch: 'dev', url: 'https://github.com/tolux17tech/demo-counter-app.git'
                }
            }
        }
    }
}