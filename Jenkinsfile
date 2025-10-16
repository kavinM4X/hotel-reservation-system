pipeline {
  agent any
  environment {
    APP_SERVER_IP = "13.127.106.132"
    APP_USER = "ec2-user"
    DEPLOY_SSH = "deploy-ssh"       // Jenkins SSH credential ID
  }

  stages {
    stage('Deploy to App Server') {
      steps {
        sshagent(credentials: [env.DEPLOY_SSH]) {
          sh "ssh -o StrictHostKeyChecking=no ${APP_USER}@${APP_SERVER_IP} '~/app/deploy.sh'"
        }
      }
    }

    stage('Finish') {
      steps {
        echo "Deployment complete! Visit http://${APP_SERVER_IP}:3000 to view your website."
      }
    }
  }
}
