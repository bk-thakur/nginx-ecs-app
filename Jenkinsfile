pipeline {
  agent any

  environment {
    AWS_REGION = "ap-south-1"
    ECR_REPO = "331760067638.dkr.ecr.ap-south-1.amazonaws.com/nginx-ecs-app"
    CLUSTER = "nginx-ecs-app"
    SERVICE = "my-replica"
  }

  stages {

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t nginx-app .'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 331760067638.dkr.ecr.ap-south-1.amazonaws.com
        '''
      }
    }

    stage('Push Image to ECR') {
      steps {
        sh '''
        docker tag nginx-app:latest $ECR_REPO:latest
        docker push $ECR_REPO:latest
        '''
      }
    }

    stage('Deploy to ECS') {
      steps {
        sh '''
        aws ecs update-service \
        --cluster $CLUSTER \
        --service $SERVICE \
        --force-new-deployment
        '''
      }
    }
  }
}
