pipeline {
  agent any
  environment {
    APP_SERVER_IP = "13.127.106.132"
    APP_USER = "ec2-user"
    DEPLOY_SSH = "deploy-ssh"    // Jenkins SSH credential ID
    REPO_URL = "https://github.com/kavinM4X/hotel-reservation-system.git"
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: env.REPO_URL, credentialsId: 'git-credentials' // if repo private
      }
    }
    stage('Deploy to App Server') {
      steps {
        sshagent(credentials: [env.DEPLOY_SSH]) {
          sh """
            ssh -o StrictHostKeyChecking=no ${APP_USER}@${APP_SERVER_IP} 'mkdir -p ~/app && cd ~/app && git pull || git clone ${REPO_URL} .'
          """
        }
      }
    }
  }
}
