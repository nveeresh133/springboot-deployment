pipeline {
 agent any
 environment {
 AWS_ACCOUNT_ID="823226410025"
 AWS_DEFAULT_REGION="us-east-2" 
 IMAGE_REPO_NAME="spring-private"
 IMAGE_TAG="latest"
 REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
 }
 
 stages {
 
 stage('Logging into AWS ECR') {
 steps {
 script {
 sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
//   aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 823226410025.dkr.ecr.us-east-2.amazonaws.com
 }
 
 }
 }
 
 stage('Clone repository') { 
            steps { 
                script{
                git credentialsId: 'github-auth', url: 'https://github.com/nveeresh133/springboot-deployment.git'
                }
            }
        }
 
 // Building Docker images
 stage('Building image') {
 steps{
 script {
 dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
 }
 }
 }
 
 // Uploading Docker images into AWS ECR
 stage('Pushing to ECR') {
 steps{ 
 script {
 sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
 sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
 }
 }
 }
 }
}
