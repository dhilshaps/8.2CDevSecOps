pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')   // Add SonarCloud token in Jenkins credentials with ID: SONAR_TOKEN
    }

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
        stage('SonarCloud Analysis') {
            steps {
                sh '''
                    # Download SonarScanner CLI
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                    unzip -q sonar-scanner.zip -d .
                    export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin

                    # Run SonarScanner with your project details
                    sonar-scanner \
                      -Dsonar.projectKey=dhilshaps_8.2CDevSecOps \
                      -Dsonar.organization=dhilshaps \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                '''
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

