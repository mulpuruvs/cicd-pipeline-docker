pipeline {
    agent any
    
    stages {
        
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
        stage('Build Docker image') {
    		when {
    			branch 'master'
    		}
    		steps {
    			script {
    				app = docker.build("mulpuruvs/jenkins-app")
    				app.inside {
    					sh 'echo $(curl 54.212.127.94:8080)'
    				}
    
    			}
    		}	
		
	    }

	    stage('Push Docker image') {
    		when {
    			branch 'master'
    		}
    		steps {
    			script {
    				docker.withRegistry('https://registry.hub.docker.com', 'DocCred') {
                        	app.push('${env.BUILD_NUMBER') 
    				    app.push('latest')
    				}
    			}
		}	
		
	}
    }        
}
