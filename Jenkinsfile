pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'chnolann@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Stage 1: Build using Maven'
                // sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Stage 2: Running JUnit and TestNG tests'
                // sh 'mvn test'
            }
            post {
                always {
                    script {
                        def status = currentBuild.currentResult
                        emailext (
                            subject: "Test Stage: ${status}",
                            body: "The Unit and Integration Test stage has completed with status: ${status}",
                            to: "chnolann@gmail.com",
                            from: "chnolann@gmail.com",
                            attachmentsPattern: '**/target/surefire-reports/*.txt'
                        )
                    }
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Stage 3: Code analysis with SonarQube'
                // sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Stage 4: Security scanning with OWASP Dependency-Check'
                // sh './dependency-check.sh --project MyApp --scan . --format ALL'
            }
            post {
                always {
                    script {
                        def status = currentBuild.currentResult
                        emailext (
                            subject: "Security Scan Stage: ${status}",
                            body: "The Security Scan stage has completed with status: ${status}",
                            to: "${env.EMAIL_RECIPIENT}",
                            from: "chnolann@gmail.com",
                            attachmentsPattern: '**/dependency-check-report.*',
                            mimeType: 'text/html'
                        )
                    }
                }
            }
        } // ← THIS CLOSING BRACE WAS MISSING

        stage('Deploy to Staging') {
            steps {
                echo 'Stage 5: Deploy to Staging using AWS CLI'
                // sh 'aws deploy push ...'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Stage 6: Selenium tests on staging environment'
                // sh './run-selenium-tests.sh'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Stage 7: Deploy to Production using AWS CLI'
                // sh 'aws deploy push ...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
