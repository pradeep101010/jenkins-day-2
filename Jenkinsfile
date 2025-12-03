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

        stage('Checkout Main Branch') {
            steps {
                script {
                    deleteDir()
                    sh '''
                        git clone --branch master --single-branch https://github.com/heroku/node-js-sample.git .
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests (Skip If None)') {
            steps {
                script {
                    def hasTestScript = sh(
                        script: "jq -r '.scripts.test // \"\"' package.json | grep -v '^null$'",
                        returnStatus: true
                    ) == 0

                    if (hasTestScript) {
                        echo "Running tests..."
                        sh 'npm test'
                    } else {
                        echo "No test script in package.json — skipping tests."
                    }
                }
            }
        }

        stage('Build App (Skip If None)') {
            steps {
                script {
                    def hasBuildScript = sh(
                        script: "jq -r '.scripts.build // \"\"' package.json | grep -v '^null$'",
                        returnStatus: true
                    ) == 0

                    if (hasBuildScript) {
                        echo "Running build..."
                        sh 'npm run build'
                    } else {
                        echo "No build script — creating dummy build directory"
                        sh "mkdir -p build && cp -r * build/ || true"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def targetDir = "${DEPLOY_DIR}/${params.ENVIRONMENT}"
                    sh """
                        mkdir -p ${targetDir}
                        cp -r build/* ${targetDir}/
                    """
                    echo "Deployment completed to: ${targetDir}"
                }
            }
        }

    }
}
