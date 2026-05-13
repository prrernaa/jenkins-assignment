pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/prrernaa/jenkins-assignment.git'
            }
        }

        stage('Install Express Dependencies') {
            steps {
                dir('express-app') {
                    sh 'npm install'
                }
            }
        }

        stage('Install Flask Dependencies') {
            steps {
                dir('flask-app') {
                    sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Restart Express App') {
            steps {
                sh '''
                pm2 delete express-app || true
                pm2 start express-app/server.js --name express-app
                '''
            }
        }

        stage('Restart Flask App') {
            steps {
                sh '''
                pm2 delete flask-app || true
                pm2 start "flask-app/venv/bin/python flask-app/app.py" --name flask-app
                '''
            }
        }

    }
}
