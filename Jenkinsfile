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

        stage('2. Cloning Repository')
          steps {
            echo '2. Cloning Repository'
            git branch: "${GIT_TARGET_BRANCH}"
                credentials: "${GIT_CREDENTIONALS_ID}"
                url: "${GIT_REPOSITORY_URL}"

          }
    }    
}
