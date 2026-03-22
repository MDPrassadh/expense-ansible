pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/expense-app.git'
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Deploy Backend') {
            steps {
                dir('backend') {
                    sh 'pm2 delete backend || true'
                    sh 'pm2 start index.js --name backend'
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                dir('frontend') {
                    sh 'cp -r build/* /var/www/html/'
                }
            }
        }

        stage('Database Setup') {
            steps {
                dir('db') {
                    sh 'mysql -h mysql.jioairlines.online -u expense -pExpenseApp@1 transactions < transactions.sql'
                }
            }
        }
    }
}
