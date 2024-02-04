pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REPOSITORY = 'public.ecr.aws/r7m7o9d4/itay9413'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                    sh "echo 'FROM alpine' | docker build -t ${ECR_REPOSITORY} -"
                    sh "docker tag ${ECR_REPOSITORY} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}"
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed to ECR successfully'
        }
        failure {
            echo 'Docker image build or push to ECR failed'
        }
    }
}
