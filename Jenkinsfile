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
                            cd voting-app
                             mvn clean package
                             mvn clean install
                             mvn -B verify
                            '''
                 }
            }
            // Build The Project              
            stage('Build') {
                         steps {
                             git 'https://github.com/La-litha/voting-app.git'
                           bat '''
                            cd voting-app
                            mvn clean install
                           '''
                         }
        }
          stage('SonarQube') {
            steps{
                bat '''
                    cd voting-app
                    mvn sonar:sonar \
                     -Dsonar.projectKey=Happy-Trip \
                     -Dsonar.host.url=http://localhost:9000 \
                     -Dsonar.login=b5c2c3f618a864f53fe1b11d828705a98f2f887d
                '''
            } 
        }
        stage('Approval') {
            agent none
            steps {
                script {
                    def deploymentDelay = input id: 'Deploy', message: 'Deploy to production?', submitter: 'rkivisto,admin', parameters: [choice(choices: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'], description: 'Hours to delay deployment?', name: 'deploymentDelay')]
                    sleep time: deploymentDelay.toInteger(), unit: 'HOURS'
                }
            }
        }
        stage('Build') { 
        	tools {	
        			jdk 'jdk 1.8'
        			maven 'apache-maven-3.6.3'
        	    }
    		steps {
    		    bat '''
    		        cd voting-app
            		    java -version
            			mvn -version
            			mvn clean package
                	'''
    			}
    		}
    		
        
    	stage('Deploy') {
    		steps{
    			echo "Deploying"
    			        deploy adapters: [tomcat7(credentialsId: '21e30ec6-0d46-4ea8-9078-ef90a528a39f', path: '', url: 'http://localhost:8093')], contextPath: 'HappyTrip', war : '**/*.war'
    			}

    }
  }      
}   
