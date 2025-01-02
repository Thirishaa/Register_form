pipeline {
    agent any

    tools {
        nodejs 'nodejs_20.10.0' // Ensure Node.js is installed and configured in Jenkins
    }

    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs'
        SONAR_SCANNER_PATH = 'C:/Program Files/sonar-scanner-6.2.1.4610-windows-x64/bin'
        SONAR_TOKEN = credentials('sonar-token') // Access SonarQube token stored in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the SCM (e.g., Git)
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Set PATH and install Node.js dependencies
                bat """
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                """
            }
        }

        stage('Lint') {
            steps {
                // Run linting to ensure code quality
                bat """
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint
                """
            }
        }

        stage('Build') {
            steps {
                // Build the React app
                bat """
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                """
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Ensure sonar-scanner is in the PATH and perform SonarQube analysis
                bat """
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=sonar-web -Dsonar.login=%SONAR_TOKEN%
                """
            }
        }
    }
}
