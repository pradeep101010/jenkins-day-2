pipeline {
    agent { label 'docker-agent' }
    
    options {
        skipDefaultCheckout()  // skip automatic branch checkout
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
                    // Completely clean workspace before checkout
                    deleteDir()

                    // Checkout main branch explicitly
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        doGenerateSubmoduleConfigurations: false,
                        userRemoteConfigs: [[
                            url: 'https://github.com/heroku/node-js-sample.git',
                            refspec: '+refs/heads/main:refs/remotes/origin/main'
                        ]]
                    ])
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
