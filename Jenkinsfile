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
        DEPLOY_DIR = "/MYAPP"
    }

    stages {

        stage('Print Selected Environment') {
            steps {
                echo "Running pipeline for environment: ${params.ENVIRONMENT}"
            }
        }

        stage('Checkout Code') {
            steps {
                script {
                    deleteDir()
                    sh '''
                        git clone --branch master --single-branch https://github.com/heroku/node-js-sample.git .
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def targetDir = "${DEPLOY_DIR}/${params.ENVIRONMENT}"
                    echo "Deploying to: ${targetDir}"

                    sh """
                    sudo mkdir -p ${targetDir}
                    sudo cp -r * ${targetDir}/
                    """
                }
            }
        }
    }
}
