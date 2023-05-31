pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                git credentialsId: 'github-auth', url: 'https://github.com/nveeresh133/springboot-deployment.git'
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("veeresh133/newrepodocker")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                script{
                        docker.withRegistry('https://823226410025.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:AWS Credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
