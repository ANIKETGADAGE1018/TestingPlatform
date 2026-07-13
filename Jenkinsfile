pipeline{
    agent any
    environment{
        mysql_user = 'root'
        mysql_password = 'root123'
        Global_Credential = credentials('Global_Credential')
        System_Credential = credentials('System_Credential')
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
                     echo "This is Mysql User Name : ${mysql_user}"
                     echo "This is Mysql Password : ${env.mysql_password}"
                }
            }
            stage('Deploy'){
                steps{
                    echo 'Deploying the application...'
                    echo "This is Git Commit ID : ${env.GIT_COMMIT}"
                    echo "This is Global Credential : ${env.Global_Credential}"
                    echo "This is System Credential : ${env.System_Credential}"
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
