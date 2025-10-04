pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/dhilshaps/8.2CDevSecOps.git'
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
                to: 'dhilshaps7@gmail.com',
                subject: "Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """The build for ${env.JOB_NAME} has finished with status: ${currentBuild.currentResult}.

Check the console log at ${env.BUILD_URL} for details.""",
                attachLog: true
            )
        }
    }
}

