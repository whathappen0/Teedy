pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from a version control system
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Build your application
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                // Run tests and generate the sure-fire report
                sh 'mvn test'
            }
            post {
                always {
                    // Archive the sure-fire reports
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', fingerprint: true
                }
            }
        }

        stage('Generate Javadoc') {
            steps {
                // Generate javadoc
                sh 'mvn javadoc:javadoc --fail-never'
            }
            post {
                always {
                    // Archive the javadoc
                    archiveArtifacts artifacts: '**/target/site/apidocs/**/*', fingerprint: true
                }
            }
        }
    }
    
    post {
        // Email notifications, cleanup, or other steps could be added here
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
