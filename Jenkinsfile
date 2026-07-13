def df
pipeline {
    agent any

    environment {
        mysql_user = 'root'
        mysql_password = 'root123'
        Global_Username = credentials('global')
    }
    parameters {
        string(name: 'VERSION', defaultValue: '1.12.0', description: 'Version to build')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Select the environment to deploy')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests after build')   
     }

    stages {

        stage('initialization') {
            steps {
                echo 'Checking out the code...'
                echo "This is Git Version Name: ${VERSION}"
                echo "This is Git ENVIRONMENT ID: ${env.ENVIRONMENT}"
                echo "This is RUN_TESTS status: ${env.RUN_TESTS}"
            }
        }

        stage('Build') {
            when {
                expression {
                    return env.BRANCH_NAME == 'Develop'
                }
            }
            steps {
                echo 'Building the application...'
                echo "This is Branch Name: ${env.BRANCH_NAME}"
            }
        }

        stage('Test') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
            }
            script {
                def script = load 'script.groovy'
                script.Test()
            }
            steps {
                echo 'Testing the application...'
                echo "This is Git Branch Name: ${env.GIT_BRANCH}"
                echo "This is MySQL User Name: ${env.mysql_user}"

                // Avoid printing passwords in production
                echo "This is MySQL Password: ${env.mysql_password}"
                echo "This is Global Username : ${Global_Username_USR}"
                echo "This is Global Username : ${Global_Username_PSW}"
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
            }
            script {
                def script = load 'script.groovy'
                script.Deploy()
            }
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'myusername',
                        usernameVariable: 'GLOBAL_USERNAME',
                        passwordVariable: 'GLOBAL_PASSWORD'
                    )
                ]) {
                    echo "This is Global Username: ${GLOBAL_USERNAME}"

                    // Don't print passwords in real pipelines
                    echo "This is Global Password: ${GLOBAL_PASSWORD}"
                }

                echo 'Deploying the application...'
                echo "This is Git Commit ID: ${env.GIT_COMMIT}"
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages are complete.'
        }

        success {
            echo 'This will run only if the pipeline succeeds.'
        }

        failure {
            echo 'This will run only if the pipeline fails.'
        }
    }
}
