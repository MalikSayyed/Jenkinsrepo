pipeline {
    agent any

    environment {
        // Replace these with your actual values
        AWS_CREDENTIALS = credentials('jenkins-aws')
        GIT_CREDENTIALS = credentials('jenkins-github-ssh')
        S3_BUCKET = 'testingblog.info'            // Your S3 bucket name
        CLOUDFRONT_DISTRIBUTION_ID = 'E2FFJ6J5AXA2PB'    // Your CloudFront distribution ID
        REGION = 'us-east-1'                          // Your AWS region (e.g., us-west-2)
        WEBSITE_DIRECTORY = './build'                 // Your  website directory after build
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'git@github.com:maliksayyed/Jenkinsrepo.git',
                    credentialsId: 'jenkins-github-ssh',
                    branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Example build command, replace with your own if different
                sh 'npm install && npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Example test command, replace with your own if different
                sh 'npm test'
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    sh '''
                    aws s3 sync $WEBSITE_DIRECTORY s3://$S3_BUCKET --delete \
                        --region $REGION \
                        --access-key $AWS_CREDENTIALS_USR \
                        --secret-key $AWS_CREDENTIALS_PSW
                    '''
                }
            }
        }

        stage('Invalidate CloudFront') {
            steps {
                script {
                    sh '''
                    aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                        --paths "/*" \
                        --region $REGION \
                        --access-key $AWS_CREDENTIALS_USR \
                        --secret-key $AWS_CREDENTIALS_PSW
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}