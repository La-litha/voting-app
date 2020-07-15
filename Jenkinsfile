pipeline {
    agent any
        stages {
            stage('package'){
                steps{
                        bat '''
                            cd VotingApp
                             mvn clean install
                            '''
                 }
            }
            
          stage('SonarQube') {
            steps{
                bat '''
                    cd VotingApp
                    mvn sonar:sonar \
                     -Dsonar.projectKey=voting-app \
                     -Dsonar.host.url=http://localhost:9000 \
                     -Dsonar.login=e5f081032c178e543854c9449715bf691cdf9c4e
                '''
            }
        }
	// Build The Project              
            stage('Build Maven') { 
        	tools {	
        			jdk 'jdk 1.8'
        			maven 'apache-maven-3.6.3'
        	    }
    		steps {
    		    bat '''
    		        cd VotingApp
            		    java -version
            			mvn -version
            			mvn clean package
                	'''
    			}
    		}
    }
        
}   

