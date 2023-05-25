pipeline{
    agent any
    tools{
        maven "maven"
    }
    stages{
        stage('Git Checkout from SCM'){
            steps{
                script{
                    git branch: 'docker', url: 'https://github.com/tolux17tech/demo-counter-app.git'
                }
            }
        }
    
        stage("Docker Image Build"){
            steps{
                script{

                    withCredentials([usernamePassword(credentialsId: 'docker-hub-auth', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "docker build -t $JOB_NAME:v1.$BUILD_ID ." 
                    sh "docker tag $JOB_NAME:v1.$BUILD_ID tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                    sh "docker push tolux17tech/$JOB_NAME:v1.$BUILD_ID"
                    }
                  
                }
            }
        }
        }
    }
