pipeline{
    agent any
    environment{
        mysql_user = 'root'
        mysql_password = 'root123'
    }
        stages{
            stage('Build'){
                when{
                    expression{
                        return BRANCH_NAME == 'Develop'
                    }
                }
                steps{
                    echo 'Building the application...'
                    echo "This is Branch Name : ${BRANCH_NAME}"
                }
            }
            stage('test'){
                when{
                    expression{
                        return BRANCH_NAME == 'main'
                    }
                }
                steps{
                    echo 'Testing the application...'
                    echo "This is Git Branch Name : ${env.GIT_BRANCH}"
                     echo "This is Mysql User Name : ${env.mysql_user}"
                     echo "This is Mysql Password : ${mysql_password}"
                }
            }
            stage('Deploy'){
                steps{
                    echo 'Deploying the application...'
                    echo "This is Git Commit ID : ${env.GIT_COMMIT}"
                }
            }
        }
        post{
            always{
                echo 'This will always run after the stages are complete.'
            }
            success{
                echo 'This will run only if the pipeline succeeds.' 
            }
            failure{
                echo 'This will run only if the pipeline fails.'
            }
        }
    
}
