pipeline{
    agent any
        stages{
            stage('Build'){
                steps{
                    echo 'Building the application...'
                    echo "This is Branch Name : ${BRANCH_NAME}"
                }
            }
            stage('test'){
                steps{
                    echo 'Testing the application...'
                    echo "This is Git Branch Name : ${GIT_BRANCH}"
                }
            }
            stage('Deploy'){
                steps{
                    echo 'Deploying the application...'
                    echo "This is Git Commit ID : ${GIT_COMMIT}"
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
