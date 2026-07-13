pipeline {
    agent any

    environment {
        mysql_user = 'root'
        mysql_password = 'root123'
        Global_Username = credentials('global')
    }

    stages {

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
            steps {
                echo 'Testing the application...'
                echo "This is Git Branch Name: ${env.GIT_BRANCH}"
                echo "This is MySQL User Name: ${env.mysql_user}"

                // Avoid printing passwords in production
                echo "This is MySQL Password: ${env.mysql_password}"
                echo "This is Global Username : ${Global_Username}"
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
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
