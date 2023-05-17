pipeline {
    agent any
    environment {
        INSTANCE_IP = '10.1.122.111'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set default.region $AWS_DEFAULT_REGION
                        scp -o StrictHostKeyChecking=no -i /path_to_key.pem -r ./ ec2-user@$INSTANCE_IP:/home/ec2-user/
                        ssh -o StrictHostKeyChecking=no -i /path_to_key.pem ec2-user@$INSTANCE_IP 'cd /home/ec2-user && npm install && npm run start'
                    '''
                }
            }
        }
    }
}
