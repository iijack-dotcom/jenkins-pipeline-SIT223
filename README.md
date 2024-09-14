pipeline {
    agent any
    environment {
        DIRECTORY_PATH = '/path/to/code'
        TESTING_ENVIRONMENT = 'staging'
        PRODUCTION_ENVIRONMENT = 'Jack-Production' 
        NOTIFICATION_EMAIL = 'cchhiv@deakin.edu.au'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the project using Maven in ${DIRECTORY_PATH}"
                // Command: mvn clean install
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "Running unit and integration tests using JUnit"
                // Command: mvn test
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Analyzing code quality using SonarQube"
                // Command: sonar-scanner
            }
        }

        stage('Security Scan') {
            steps {
                echo "Performing security scan using OWASP Dependency-Check"
                // Command: dependency-check
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to staging environment: ${TESTING_ENVIRONMENT}"
                // Example: scp files to AWS EC2 or use a deployment script
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests on the staging environment"
                // Command: mvn verify -Pstaging
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to production environment: ${PRODUCTION_ENVIRONMENT}"
                // Example: scp files to production server or run a deployment script
            }
        }
    }

    post {
        always {
            echo "Sending email notification"
            mail to: "${NOTIFICATION_EMAIL}",
                 subject: "Pipeline Stage Completed",
                 body: "The Jenkins pipeline has completed.",
                 attachLog: true
        }
    }
}
