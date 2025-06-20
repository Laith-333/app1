pipeline {
    agent any

    environment {
        TEST_IP = '192.168.124.153'
        PROD_IP = '192.168.124.145'
        TEST_PASSWORD = credentials('student-password')
        PROD_PASSWORD = credentials('student-password2')
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
                sh '''
                sshpass -p "$TEST_PASSWORD" scp -o StrictHostKeyChecking=no -r Dockerfile Jenkinsfile index.html student@$TEST_IP:/var/www/html/
                '''
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
                sh '''
                sshpass -p "$PROD_PASSWORD" scp -o StrictHostKeyChecking=no -r Dockerfile Jenkinsfile index.html student@$PROD_IP:/var/www/html/
                '''
            }
        }
    }
}
