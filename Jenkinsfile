pipeline {
  agent any

  environment {
    AWS_DEFAULT_REGION = 'us-east-1'
    S3_BUCKET          = 'my-static-site-bucket'
  }

  stages {
    stage('Validate') {
      steps {
        echo 'Validating static files...'
      }
    }

    stage('Deploy to S3') {
      steps {
        withAWS(credentials: 'aws-credentials') {
          sh """
            aws s3 sync ./ s3://${S3_BUCKET}/ \
              --exclude "Jenkinsfile" \
              --exclude ".git/*" \
              --delete
          """
        }
      }
    }
  }

  post {
    success {
      echo "Deployed successfully to s3://${S3_BUCKET}/"
    }
    failure {
      echo 'Deployment failed.'
    }
  }
}
