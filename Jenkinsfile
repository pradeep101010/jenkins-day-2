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
                echo "Running pipeline for environment: ${params.ENVIRONMENT}"
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
                    def targetDir = "${DEPLOY_DIR}/${params.ENVIRONMENT}"

                    echo "Deploying to: ${targetDir}"

                    sh """
                        mkdir -p ${targetDir}
                        cp -r build/* ${targetDir}/
                    """
                }
            }
        }
    }
}
