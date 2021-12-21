
pipeline{
    agent any

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
                    sh "aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 489994096722.dkr.ecr.us-west-1.amazonaws.com"
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




        stage("Which ECS"){
            steps{
                script{
                    sh "which ecs"
                }
            }
        }

        stage("Version ECS"){
            steps{
                script{
                    sh 'cd /home/ubuntu/.local/bin/'
                    sh "ecs -v"
                }
            }
        }


        stage("Create a Task"){
            steps{
                script{
                    sh "ecs run SharjeeLcluster sharjeel_task_def --region us-west-1"
                }
            }
        }


        stage("Update Service"){
            steps{
                script{
                    sh "ecs deploy SharjeeLcluster sharjeelservice"
                }
            }
        }


    }

}