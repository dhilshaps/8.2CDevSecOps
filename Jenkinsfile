pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/dhilshaps/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }
    }

    post {
        always {
            emailext (
                to: 'dhilshapspromos@gmail.com',
                subject: "Build #${BUILD_NUMBER} - ${BUILD_STATUS}",
                body: """The build for ${JOB_NAME} has finished with status: ${BUILD_STATUS}.

Check the console log at ${BUILD_URL} for details.""",
                attachLog: true
            )
        }
    }
}

