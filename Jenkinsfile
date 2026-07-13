pipeline{
    agent any{
        stages{
            stage('Build'){
                steps{
                    echo 'Building the application...'
                    sh 'Build command goes here'
                }
            }
            stage('test'){
                steps{
                    echo 'Testing the application...'
                }
            }
            stage('Deploy'){
                steps{
                    echo 'Deploying the application...'
                }
            }
        }
        post{
            always{
                echo 'This will always run after the stages are complete.'
            }
            sucess{
                echo 'This will run only if the pipeline succeeds.' 
            }
            failure{
                echo 'This will run only if the pipeline fails.'
            }
        }
    }
}
