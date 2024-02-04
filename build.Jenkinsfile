pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REPOSITORY = 'public.ecr.aws/r7m7o9d4/itay9413'
        DOCKERFILE_PATH = 'Dockerfile'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                    def dockerImageTag = "${ECR_REPOSITORY}:0.0.${BUILD_NUMBER}"
                    sh "docker build -t ${dockerImageTag} -f ${DOCKERFILE_PATH} ."
                    sh "docker tag ${dockerImageTag} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${BUILD_NUMBER}"
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        success {
            echo "Docker image built and pushed to ECR successfully with tag: ${BUILD_NUMBER}"
        }
        failure {
            echo 'Docker image build or push to ECR failed'
        }
    }
}
