pipeline {
    agent any
             options {
                    // Keep the 10 most recent builds
                buildDiscarder(logRotator(numToKeepStr:'10')) 
            }
        stages {
            stage('package'){
                steps{
                        bat '''
                            cd HappyTrip
                             mvn clean package
                             mvn clean install
                             mvn -B verify
                            '''
                 }
            }
            // Build The Project              
            stage('Build') {
                         steps {
                             git 'https://github.com/La-litha/Test_Happytrip.git'
                           bat '''
                            cd Happytrip
                            mvn clean install
                           '''
                        }
                  post {
                       success{
                              archiveArtifacts(artifacts: 'HappyTrip/reports/*.html', allowEmptyArchive: true)
                       }
                }
        }
    }          
    post{
                failure{
                    mail to: 'padmajothi020296@gmail.com', from: 'padmajothi020296@gmail.com',
                    body: "Job Failed - \"${env.JOB_NAME}\" build: ${env.BUILD_NUMBER}",
                    subject: "Project Build: ${env.JOB_NAME} - Failed"
                        
                }
                success {
                    emailext attachmentsPattern: 'reports/.html', 
                    body: '''${SCRIPT, template="groovy-html.template"}''',
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - successful",
                    mimeType: 'text/html', 
                    to:"padmajothi020296@gmail.com"
                        
                }
        }   
}
