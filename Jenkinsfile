pipeline {
    agent { label 'docker-agent' }
    options {
        skipDefaultCheckout()
    }

    parameters {
        string(
            name: 'ENVIRONMENT',
            defaultValue: 'dev',
            description: 'Choose environment: dev / test / prod'
        )
    }

    environment {
        DEPLOY_DIR = "/var/www/myapp"
    }

    stages {

        stage('Print Selected Environment') {
            steps {
                echo "Running pipeline for environment: ${params.ENVIRONMENT}"
            }
        }

        stage('Checkout') {
            steps {
                // Clean workspace to avoid branch conflicts
                deleteDir()
                
                // Explicitly checkout Heroku main
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'refs/heads/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/heroku/node-js-sample.git',
                        refspec: '+refs/heads/main:refs/remotes/origin/main'
                    ]]
                ])
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
