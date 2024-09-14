pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Starting Build Stage...'
                // Use Maven 
                echo 'sh mvn clean install'
                echo 'Build Stage Completed!'
            }
	 post {
            always{
            // Send email notification on pipeline success
                mail to: "bongthom024@gmail.com",
                subject: "The pipeline has completed successfully",
                body: "Build stage complete!"
            }
        }
    
	}
        stage('Unit and Integration Tests') {
            steps {
                echo 'Starting Unit and Integration Tests Stage...'
                // Run unit and integration tests
                echo 'sh mvn test'
                  // Run Postman/Newman tests for API
                echo 'Unit and Integration Tests Stage Completed!'
            }
        

	 post {
            always{
            // Send email notification on Tests success
                mail to: "bongthom024@gmail.com",
                subject: "The Test is completed successfully",
                body: "Test stage complete!"
            }
        }
    }

        stage('Code Analysis') {
            steps {
                echo 'Starting Code Analysis Stage...'
                 // Integrate with SonarQube for code analysis
                // SonarQube scanner command 
                echo 'Code Analysis Stage Completed!'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Starting Security Scan Stage...'
                 // OWASP ZAP for security scanning
                // ZAP scanner command 
                echo 'Security Scan Stage Completed!'
            }
        

	 post {
            always{
            // Send email notification on Security Scan success
                mail to: "bongthom024@gmail.com",
                subject: "The Security Scan is completed successfully",
                body: "Security Scan complete!"
            }
        }
    }

        stage('Deploy to Staging') {
            steps {
                echo 'Starting Deploy to Staging Stage...'
                 // AWS Elastic Beanstalk or AWS CodeDeploy to deploy to staging
                // deployment commands for staging 
                echo 'Deploy to Staging Stage Completed!'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Starting Integration Tests on Staging Stage...'
                  // Run integration tests on the staging environment
                //Postman for API testing
                // testing commands
                echo 'Integration Tests on Staging Stage Completed!'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Starting Deploy to Production Stage...'
                 // AWS Elastic Beanstalk or AWS CodeDeploy to deploy to production
                // deployment commands for production 
                echo 'Deploy to Production Stage Completed!'
            }
        }
    
    }
	post{
		always{
			emailext attachLog: true,
			to: "bongthom024@gmail.com",
			subject: "The pipeline has completed successfully",
			body: "	Final Build log attached!"
		}
	}
}
