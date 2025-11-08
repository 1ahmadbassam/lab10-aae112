pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat 'python -m venv %VIRTUAL_ENV%'
                    }
                }
                bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && pip install -r requirements.txt'
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && flake8 app.py'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && python -m coverage run -m pytest'
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    echo 'Running Code Coverage...'
                    // Generate a console report and an XML report for CI tools
                    bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && python -m coverage report'
                    bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && python -m coverage xml'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Running Security Scan...'
                    bat '%VIRTUAL_ENV%\\Scripts\\activate.bat && bandit -r .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Implement a simple local deployment
                    echo "Deploying application to local 'dist' folder..."

                    // Create a 'dist' directory (if it doesn't exist)
                    bat 'if not exist dist mkdir dist'

                    // Copy the application file to the 'dist' directory
                    bat 'copy app.py dist\\app.py'

                    echo 'Deployment complete.'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
