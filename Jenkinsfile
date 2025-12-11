pipeline {
    agent any
    
  environment {
        TIME_ZONE = 'Asia/Seoul'

        // GitHub
        GIT_TARGET_BRANCH = 'main'
        GIT_REPOSITORY_URL = 'https://github.com/LSH-3016/fastapp'
        GIT_CREDENTIONALS_ID = 'git_cre'

        // AWS ECR
        AWS_ECR_CREDENTIAL_ID = 'aws_cre'
        AWS_ECR_URI = '324547056370.dkr.ecr.ap-northeast-2.amazonaws.com'
        AWS_ECR_IMAGE_NAME = 'fast'
        AWS_RESION = 'ap-northeast-2'
    }
    
    stages {
        stage('1. init') {
            steps {
                echo "1. init stage"
                deleteDir()
            }
        }

        stage('2. Cloning Repository'){
          steps {
            echo '2. Cloning Repository'
            git branch: "${GIT_TARGET_BRANCH}",
                credentials: "${GIT_CREDENTIONALS_ID}",
                url: "${GIT_REPOSITORY_URL}"

            }
        }

        stage('3. Build Docker Image'){
            steps{
                script{
                    sh '''
                        docker build -t ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:${BUILD_NUMBER} .
                        docker build -t %{AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:latest .
                    '''
                }
            }
        }

        stage('4.Push to ECR') {
            steps {
              // aws credential 플러그인을 설치해서 사용할 수 있는 함수. aws configure와 같은 기능.
              withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_ECR_CREDENTIAL_ID}"]]) {
                    script {
                        sh '''
                        aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ECR_URI}
                        docker push ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:latest
                        '''
                    }
                }
            }
            post {
                failure {
                    script {
                        sh '''
                        docker rm -f ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:${BUILD_NUMBER}
                        docker rm -f ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:latest
                        echo docker image push fail
                        '''
                    }
                }
                success {
                    script {
                   
                        sh '''
                        docker rm -f ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:${BUILD_NUMBER}
                        docker rm -f ${AWS_ECR_URI}/${AWS_ECR_IMAGE_NAME}:latest
                        echo docker image push success
                        '''
                    }

                }                
            }
        }


    }    

}
