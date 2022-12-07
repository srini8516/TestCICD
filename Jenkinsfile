pipeline{
    agent any
       stages {
	     stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/srini8516/TestCICD.git'
                }
            }
        }
       stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn verify -DskipUnitTests'
                }
            }
        }

        stage ('Build') {
            steps {
                bat 'mvn clean install' 
            }
           
        }
	       
	        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        bat 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
	       stage{
		       steps{
			       script{
			       nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']],
				       credentialsId: 'nexusauth', 
				       groupId: 'com.example', nexusUrl: '192.168.0.106:8081/', 
				       nexusVersion: 'nexus3', protocol: 'http', 
				       repository: 'demoapp-release',
				       version: '1.0.0'
			       }
		       }
	       }
		       
    }	          
}
