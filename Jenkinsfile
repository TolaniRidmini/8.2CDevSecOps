pipeline {
    agent any

    environment {
        SONAR_ORG = 'tolaniridmini-1'
        SONAR_PROJECT_KEY = 'tolaniridmini-1_8-2cdevsecops'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/TolaniRidmini/8.2CDevSecOps.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    bat """
                    npm install -g @sonar/scan
                    sonar ^
                      -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
                      -Dsonar.organization=%SONAR_ORG% ^
                      -Dsonar.host.url=https://sonarcloud.io ^
                      -Dsonar.token=%SONAR_TOKEN%
                    """
                }
            }
        }

        stage('NPM Audit') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
    }

    post {
        always {
            echo ' Pipeline completed. Check the console output for audit results and test coverage.'
        }
    }
}
