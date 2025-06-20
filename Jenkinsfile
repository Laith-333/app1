pipeline {
    agent any

    environment {
        TEST_IP = '192.168.124.153'
        PROD_IP = '192.168.124.145'
    }

    stages {
        stage('Build & Test') {
            steps {
                echo '🛠️ Building & Testing App'
                sh 'ls -la'
            }
        }

        stage('Deploy to Test') {
            steps {
                echo '🚀 Deploying to TEST'
                withCredentials([usernamePassword(credentialsId: 'student-password', usernameVariable: 'TEST_USER', passwordVariable: 'TEST_PASS')]) {
                    sh '''
                        echo "$TEST_PASS" | sshpass -p "$TEST_PASS" ssh -tt -o StrictHostKeyChecking=no $TEST_USER@$TEST_IP 'echo "$TEST_PASS" | sudo -S mkdir -p /var/www/html'
                        sshpass -p "$TEST_PASS" scp -o StrictHostKeyChecking=no Dockerfile Jenkinsfile index.html $TEST_USER@$TEST_IP:/var/www/html/
                    '''
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
                withCredentials([usernamePassword(credentialsId: 'student-password2', usernameVariable: 'PROD_USER', passwordVariable: 'PROD_PASS')]) {
                    sh '''
                        echo "$PROD_PASS" | sshpass -p "$PROD_PASS" ssh -tt -o StrictHostKeyChecking=no $PROD_USER@$PROD_IP 'echo "$PROD_PASS" | sudo -S mkdir -p /var/www/html'
                        sshpass -p "$PROD_PASS" scp -o StrictHostKeyChecking=no Dockerfile Jenkinsfile index.html $PROD_USER@$PROD_IP:/var/www/html/
                    '''
                }
            }
        }
    }
}
