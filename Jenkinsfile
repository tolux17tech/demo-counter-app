pipeline{
    agent any
    tools{
        maven 'maven'
    }
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
                    sh "mvn package"
                }
            }
        }
        stage("Testing with sonar"){
            steps{
                script{
                    sh "mvn sonar:sonar"
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
