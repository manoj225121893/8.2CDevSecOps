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
