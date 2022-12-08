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
	        stage('nexus Artifact Uploader'){
		       steps{
			       script{
				        def readpomversion = readMavenPom file: 'pom.xml'
				       def nexusRepo = readpomversion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" :"demoapp-release";
			       nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']],
				       credentialsId: 'nexusauth', 
				       groupId: 'com.example', nexusUrl: '192.168.0.106:8081/', 
				       nexusVersion: 'nexus3', protocol: 'http', 
				       repository: nexusRepo,
				       version: "${readpomversion.version}"
			       }
		       }
	       }
	  stage('Docker Image Build'){            
            steps{                
                script{
                    bat 'docker image build -t $job_name:v1.$build_id'
		    bat 'docker image tag $job_name:v1.$build_id srinivasbr0107/$job_name:v1.$build_id'
		   bat 'docker image tag $job_name:v1.$build_id srinivasbr0107/latest'	
                }
            }
        }	
	
    }	          
}
