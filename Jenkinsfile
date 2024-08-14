pipeline{
    agent any 
    environment {
    AWS_DEFAULT_REGION = "us-east-1"
    THE_BUTLER_SAYS_SO = credentials('cc5cf938-f6ed-4259-a236-690a9e1c8a8d')
    }
    stages {
        stage ('Build'){
            steps {
                echo "Building stage"
            }
        }
        stage ('Test'){
            steps {
                echo "Testing stage"

            }
        }
        stage ('Deploy to S3'){ 
            steps{ 
                echo "Deploying" 
                sh ' aws s3 cp ./index.html s3://testingblog.info '
            } 
        }

    }

    post{
        success {
            echo "success"
        }
        failure {
            echo "failure"
        }
    }
}
