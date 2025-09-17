pipeline {
    agent any
    tools {
        nodejs 'NodeJS-18'
    }
    environment {
        XDG_CONFIG_HOME = "${env.WORKSPACE}/.config"
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manoj225121893/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Snyk Test') {
            steps {
                sh './node_modules/.bin/snyk auth ${SNYK_TOKEN}'
                sh './node_modules/.bin/snyk test || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manoj225121893/8.2CDevSecOps.git'
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
            post {
                always {
                    emailext (
                        subject: "Build ${currentBuild.fullDisplayName} - Test Results: ${currentBuild.currentResult}",
                        body: "Test stage completed. See attached log.",
                        to: 'developer@example.com',
                        attachLog: true
                    )
                }
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext (
                        subject: "Build ${currentBuild.fullDisplayName} - Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan completed. Console log attached.",
                        to: 's225121893@deakin.edu.au',
                        attachLog: true
                    )
                }
            }
        }
    }
}






