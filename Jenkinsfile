pipeline {
    agent any

    environment {
        APP_SERVER_IP = "13.127.106.132"
        APP_USER = "ec2-user"
        DEPLOY_SSH = "deploy-ssh"        // Jenkins SSH credential ID for EC2
        GIT_CRED_ID = "git-credentials"  // Jenkins GitHub token credential ID
        REPO_URL = "https://github.com/kavinM4X/hotel-reservation-system.git"
        APP_PATH = "/home/ec2-user/app"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling code from GitHub..."
                git branch: 'main', url: "${REPO_URL}", credentialsId: "${GIT_CRED_ID}"
            }
        }

        stage('Deploy to App Server') {
            steps {
                echo "Deploying to EC2 App Server..."
                sshagent(credentials: [env.DEPLOY_SSH]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_USER}@${APP_SERVER_IP} '
                            if [ ! -d ${APP_PATH} ]; then
                                git clone ${REPO_URL} ${APP_PATH}
                            else
                                cd ${APP_PATH} && git pull
                            fi
                        '
                    """
                }
            }
        }

        stage('Restart Application') {
            steps {
                echo "Restarting web application on EC2..."
                sshagent(credentials: [env.DEPLOY_SSH]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_USER}@${APP_SERVER_IP} '
                            cd ${APP_PATH}
                            # Example: Start a simple web server (adjust as needed)
                            nohup python3 -m http.server 8080 > server.log 2>&1 &
                        '
                    """
                }
            }
        }

        stage('Finish') {
            steps {
                echo "✅ Deployment Complete!"
                echo "Access your app at: http://${APP_SERVER_IP}:8080"
            }
        }
    }
}
