
pipeline{
    agent any
    tools {
    dockerTool 'docker'
    }
    environment {
        registry = "489994096722.dkr.ecr.us-east-2.amazonaws.com/sharjeel"
        AWS_ACCOUNT_ID = "489994096722"
        AWS_DEFAULT_REGION = "us-east-2"
        IMAGE_REPO_NAME = "sharjeel"
        IMAGE_TAG = "apache"
        REPOSITORY_URI = '${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}'
        REVISION = 4
    }
    stages{

        stage("Login with ECR"){
            steps{
                script{
                    sh "aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 489994096722.dkr.ecr.us-east-2.amazonaws.com"
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


        stage("Whoami"){
            steps{
                script{
                    sh "whoami"
                }
            }
        }

        stage("Proceed or not"){
            steps{
                script{
                    input("Proceed or terminate")
                }
            }
        }



        stage("Update Service"){
            steps{
                script{
                    sh "ecs deploy sharjeelcluster sharjeelservice --image apache nginx:1.11.8 --timeout 600"
                }
            }
        }

    }

}