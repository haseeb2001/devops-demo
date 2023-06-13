pipeline {
      agent any

      environment {
            AWS_REGION = 'eu-north-1'
            ECR_REPOSITORY = 'myapp' // Set your Amazon ECR repository name
            ECR_REGISTRY = '009356742988.dkr.ecr.eu-north-1.amazonaws.com'
      }
      stages {
            stage('testing') {
                  steps {
                        echo 'Hi, this is testing stage for credentials'

                        script {
                              sh "echo 'My credentials: ${env.MY_CREDENTIALS}'"
                        }
                  }
            }
            stage('One') {
                  steps {
                        echo 'Hi, this is Haseeb experimenting with Jenkins'
                  }
            }

            stage('Checkout') {
                  steps {
                        checkout scm
                  }
            }
            stage('Get Git Info') {
                  steps {
                        script {
                              // Retrieve the branch name from Git
                              BRANCH_TAG = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()

                              // Retrieve the commit SHA from Git
                              IMAGE_TAG = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                              sh "echo 'IMAGE_TAG: ${IMAGE_TAG}, BRANCH_TAG: ${BRANCH_TAG}'"
                        }
                  }
            }
            stage('Login to AWS') {
                  steps {
                        script {
                              withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-access-creds-1.1'
                    ]]) {
                                    sh """
                        aws ecr get-login-password --region ${AWS_REGION} | docker login \
                        --username AWS --password-stdin ${ECR_REGISTRY}
                        """
                    }
                        }
                  }
            }
            stage('build and push to AWS') {
                  steps {
                        script {
                              withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-access-creds-1.1'
                    ]]) {
                                    // AWS CLI or SDK commands here
                                    sh """
                        docker build -t  ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BRANCH_TAG}-${IMAGE_TAG} -t \
                        ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BRANCH_TAG}-latest .
                        docker push  ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BRANCH_TAG}-${IMAGE_TAG}
                        docker push  ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BRANCH_TAG}-latest
                        echo 'image=${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}' >> $GITHUB_OUTPUT
                        """
                    }
                        }
                  }
            }
      }
}
