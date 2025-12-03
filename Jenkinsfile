pipeline {
    agent any

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

        stage('Always Checkout Heroku Master') {
            steps {
                script {

                    // Clean workspace
                    deleteDir()

                    // Always clone Heroku master regardless of current branch
                    sh '''
                        echo "Cloning Heroku master branch..."
                        git clone \
                            --branch master \
                            --single-branch \
                            https://github.com/heroku/node-js-sample.git \
                            .
                    '''
                }
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
