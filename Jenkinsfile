pipeline {
    agent any

    parameters {
        string(
            name: 'ENVIRONMENT',
            defaultValue: 'dev',
            description: 'Choose environment: dev / test / prod'
        )
    }

    environment {
        DEPLOY_DIR = "/var/www/myapp"    // Change if needed
    }

    stages {

        stage('Print Selected Environment') {
            steps {
                echo "Running pipeline for environment: ${ENVIRONMENT}"
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your/repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying build artifacts to ${ENVIRONMENT}"

                    if (ENVIRONMENT == "dev") {
                        sh "mkdir -p ${DEPLOY_DIR}/dev"
                        sh "cp -r build/* ${DEPLOY_DIR}/dev/"
                    }
                    else if (ENVIRONMENT == "test") {
                        sh "mkdir -p ${DEPLOY_DIR}/test"
                        sh "cp -r build/* ${DEPLOY_DIR}/test/"
                    }
                    else if (ENVIRONMENT == "prod") {
                        sh "mkdir -p ${DEPLOY_DIR}/prod"
                        sh "cp -r build/* ${DEPLOY_DIR}/prod/"
                    }
                    else {
                        error "Invalid environment: ${ENVIRONMENT}"
                    }
                }
            }
        }
    }
}
