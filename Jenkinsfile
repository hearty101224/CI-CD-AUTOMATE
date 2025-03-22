pipeline {
    agent any  // Runs on any available Jenkins agent

    environment {
        GITHUB_CREDENTIALS_ID = 'github-credentials' // Ensure this is set in Jenkins credentials
        REPO_URL = 'https://github.com/hearty101224/CI-CD-AUTOMATE.git'
        BRANCH_NAME = 'main'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout scmGit(
                        branches: [[name: "${BRANCH_NAME}"]],
                        userRemoteConfigs: [[
                            url: "${REPO_URL}",
                            credentialsId: "${GITHUB_CREDENTIALS_ID}"
                        ]]
                    )
                }
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Ensure Python is installed and install dependencies
                    sh 'python --version || python3 --version'
                    sh 'pip install -r requirements.txt || pip3 install -r requirements.txt'
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    echo "Running Python Application..."
                    sh 'python app.py || python3 app.py'  // Runs the Python script
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Run Python unit tests
                    sh 'pytest test_app.py --junitxml=report.xml'
                }
            }
        }

        stage('Post-Build') {
            steps {
                echo 'Build completed successfully!'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully! Check Jenkins Console Output for results.'
        }
        failure {
            echo '❌ Pipeline failed! Please check logs for errors.'
        }
    }
}
