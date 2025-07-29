pipeline {
  agent any
  environment {
    SAFE_BRANCH = env.BRANCH_NAME.replaceAll(/[^a-zA-Z0-9_.-]/, "-")
    IMAGE_NAME = "devops-hari9-auto:${SAFE_BRANCH}"
    IMAGE_LATEST = "devops-hari9-auto:latest"
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh "docker build -t ${IMAGE_NAME} ."
      }
    }

    stage('Tag as Latest (only for main)') {
      when {
        expression { env.BRANCH_NAME == 'main' }
      }
      steps {
        sh "docker tag ${IMAGE_NAME} ${IMAGE_LATEST}"
      }
    }

    stage('Run Container') {
      steps {
        sh "docker rm -f devops-auto-${SAFE_BRANCH} || true"
        sh "docker run -d --name devops-auto-${SAFE_BRANCH} -p 6002:6000 ${IMAGE_NAME}"
      }
    }

    stage('Test') {
      steps {
        sh "sleep 3 && curl localhost:6001"
      }
    }
  }
}
