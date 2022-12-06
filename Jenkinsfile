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
       

        stage ('Build') {
            steps {
                bat 'mvn clean install' 
            }
           
        }
    }	          
}
