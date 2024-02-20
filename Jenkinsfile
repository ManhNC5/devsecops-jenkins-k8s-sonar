pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=fe-lab-devsecops_lab-devsecop2024 -Dsonar.organization=fe-lab-devsecops -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=a69f96d99bfbf59668751fcb7faff94459034e43'
			}
        } 
    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
		  withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
  		            }
	   }
    }

    stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

   stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://767397696059.dkr.ecr.ca-central-1.amazonaws.com', 'ecr:ca-central-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
  

  
}
}
