pipeline {
    agent any
        stages {
            stage('package'){
                steps{
                        bat '''
                            cd worker
                             mvn clean install
                            '''
                 }
            }
            
          stage('SonarQube') {
            steps{
                bat '''
                    cd worker
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
    		        cd worker
            		    java -version
            			mvn -version
            			mvn clean package
                	'''
    			}
    		}
		  stage('Build image') {
                steps {
                    echo 'Starting to build docker image DB'
                    script {
                        def DB = docker.build("my-image:${env.BUILD_ID}","-f ${env.WORKSPACE}/db/Dockerfile .")
                        def nodejs = docker.build("my-image:${env.BUILD_ID}","-f ${env.WORKSPACE}/app/Dockerfile .") 
                        def php = docker.build("my-image:${env.BUILD_ID}","-f ${env.WORKSPACE}/php/Dockerfile .") 
                    }
                }
            }
    }
        
}   

