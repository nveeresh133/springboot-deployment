pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    
    tools {
         maven 'maven'
         jdk 'openjdk-11'
    }

    stages {
// 	 stage('Logging into AWS ECR') {
//             steps {
//                 script {
//                 sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/g5m8j8k4'
//                 }
                 
//             }
//         }
         stage('Clone repository') { 
            steps { 
                script{
                git credentialsId: 'github-auth', url: 'https://github.com/nveeresh133/springboot-deployment.git'
                }
            }
        }
        

		stage('mvn Build'){
			steps {
// 				sh '''mvn install -s settings.xml'''
				sh '''mvn install'''
			}
		}

		stage('Test'){
			steps{
				sh '''mvn test'''
			}
		}

        stage('docker Build') { 
            steps { 
                script{
                 app = docker.build("veeresh133/newrepodocker")
                }
            }
        }
//         stage('Test'){
//             steps {
//                  echo 'Empty'
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 script{
//                         docker.withRegistry('https://823226410025.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:AWS Credentials') {
//                     app.push("${env.BUILD_NUMBER}")
//                     app.push("latest")
//                     }
//                 }
//             }
//         }
	   // Uploading Docker images into AWS ECR
     	   stage('Pushing to ECR') {
     	       steps{  
         	   script {
               		sh '''docker tag veeresh133/newrepodocker:latest public.ecr.aws/g5m8j8k4/spring-boot-repo:latest1'''
              
			sh '''docker push public.ecr.aws/g5m8j8k4/veeresh133/newrepodocker:latest1'''

         }
        }
       }
    }
}
