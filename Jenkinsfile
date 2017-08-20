pipeline{
    agent any
        stages{
            stage('clone'){
                    steps {
                        timeout(time: 3, unit: 'MINUTES'){
                            retry(5){
                                echo 'Clonning the Application Source from GitHub.'
                                git(url: 'https://github.com/saikrishnabonagiri/JavaApplication.git', branch: 'master')
                                echo 'Clonning is done.'
                                
                            }
                        }
                        
                    }
                    
            }
            stage('Sanity check') {
                steps {
                    emailext (
                        to: 'sai.krishna2559@gmail.com',
                        subject: "${env.JOB_NAME} #${env.BUILD_NUMBER} [${currentBuild.result}]",
                        body: "<html><body><center><h3>Cloning the Application from GitHub is succesful. <br>Do you want to start the building of the project? <br> <br> <form action=${env.BUILD_URL}input> <input type=submit value=Approve? </form></h2></center></body><html>",
                    attachLog: true,
                    )
                    script {
                        env.COMMENT = input message: 'Start Building the Project.', parameters: [text(defaultValue: 'Clonning is Successful.', description: 'reason for proceeding to next step.', name: 'comment')]
                    }
                }
            }
            
            
            stage('build'){
                steps{
                    echo "Building the Application."
                    //sh 'ant'
                }
            }
            
            stage('notify'){
                steps {
                    try {
                    emaiext (
                        to: 'sai.krishna2559@gmail.com',
                        subject: "${env.JOB_NAME} #${env.BUILD_NUMBER} [${currentBuild.result}]",
                        body: "Build URL: ${env.BUILD_URL}/console\n\n",
                    attachLog: true,
                )
                }
                    catch (e) {
                error("Build failed because of this and that..")
            }
                }
            }
    }
}
