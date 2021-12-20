
pipeline{
    agent any
    tools {
        
    }
    environment {
        registry = "public.ecr.aws/y2a9o9h4/sharjeel"
        AWS_ACCOUNT_ID = "489994096722"
        AWS_DEFAULT_REGION = "us-west-1"
        IMAGE_REPO_NAME = "sharjeel"
        IMAGE_TAG = "latest"
        REPOSITORY_URI = '${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}'
    }
    stages{

        stage("Login with ECR"){
            steps{
                script{
                    sh "aws ecr get-login-password — region ${AWS_DEFAULT_REGION} | docker login — username AWS — password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
            }
        }

        stage("Building Image"){
            steps{
                script{
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage("Push to ECR"){
            steps{
                script{
                    sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }



    }

}
