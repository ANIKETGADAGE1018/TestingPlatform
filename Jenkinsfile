def df

pipeline {
    agent any

    environment {
        mysql_user = 'root'
        mysql_password = 'root123'
        GLOBAL_CREDS = credentials('global')
    }

    parameters {
        string(
            name: 'VERSION',
            defaultValue: '1.12.0',
            description: 'Version to build'
        )

        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select the environment'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run tests after build'
        )
    }

    stages {

        stage('Initialization') {
            steps {
                echo 'Checking out the code...'
                echo "Version: ${params.VERSION}"
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Run Tests: ${params.RUN_TESTS}"
            }
        }

        stage('Build') {
            when {
                expression {
                    env.BRANCH_NAME == 'Develop'
                }
            }

            steps {
                echo 'Building the application...'
                echo "Branch Name: ${env.BRANCH_NAME}"
            }
        }

        stage('Test') {

            when {
                expression {
                    env.BRANCH_NAME == 'main'
                }
            }

            steps {

                script {
                    def helper = load 'script.groovy'
                    helper.Test()
                }

                echo 'Testing the application...'

                echo "Git Branch : ${env.GIT_BRANCH}"
                echo "MySQL User : ${env.mysql_user}"

                echo "Credential Username : ${env.GLOBAL_CREDS_USR}"

                // Don't print passwords
                // echo "${env.GLOBAL_CREDS_PSW}"
            }
        }

        stage('Deploy') {

            when {
                expression {
                    env.BRANCH_NAME == 'main'
                }
            }

            steps {

                script {
                    def helper = load 'script.groovy'
                    helper.Deploy()
                }

                withCredentials([
                    usernamePassword(
                        credentialsId: 'myusername',
                        usernameVariable: 'GLOBAL_USERNAME',
                        passwordVariable: 'GLOBAL_PASSWORD'
                    )
                ]) {

                    echo "Username: ${GLOBAL_USERNAME}"

                    // Never print passwords
                    // echo "${GLOBAL_PASSWORD}"
                }

                echo 'Deploying the application...'
                echo "Commit ID: ${env.GIT_COMMIT}"
            }
        }
    }

    post {

        always {
            echo 'Pipeline completed.'
        }

        success {
            echo 'Pipeline succeeded.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
