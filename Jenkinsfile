pipeline {
    agent any

    environment {
        DEPLOY_USER = 'student'  // change if needed
        TEST_IP = '192.168.124.153'
        PROD_IP = '192.168.124.16'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Laith-333/Jenkins.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo '🛠️ Building & Testing App'
                sh 'ls -la app/'
            }
        }

        stage('Deploy to Test') {
            steps {
                echo '🚀 Deploying to TEST'
                sshagent (credentials: ['ssh-key-test']) {
                    sh 'scp -r app/* $DEPLOY_USER@$TEST_IP:/var/www/html/'
                }
            }
        }

        stage('Manual Approval') {
            steps {
                input message: '✅ Deploy to Production?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo '🚀 Deploying to PRODUCTION'
                sshagent (credentials: ['ssh-key-prod']) {
                    sh 'scp -r app/* $DEPLOY_USER@$PROD_IP:/var/www/html/'
                }
            }
        }
    }
}
