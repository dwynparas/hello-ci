pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        SELENIUM_URL = 'http://selenium:4444/wd/hub'
        APP_URL = 'http://jenkins:3000'
    }

    stages {
        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'npx jest tests/math.test.js --runInBand'
            }
        }

        stage('Start App') {
            steps {
                sh 'nohup node src/app.js > app.log 2>&1 &'
                sh 'sleep 5'
            }
        }

        stage('UI Test') {
            steps {
                sh 'npx jest tests/e2e/home.test.js --runInBand'
            }
        }
    }

    post {
        always {
            junit 'junit.xml'
        }
    }
}