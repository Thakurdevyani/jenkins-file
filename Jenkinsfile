pipeline {
  agent any 
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
       stage ('Check secrets') {
      steps {
      sh 'trufflehog3  https://github.com/Thakurdevyani/jenkins-file.git -f json -o truffelhog_output.json || true'
      }
    }
     stage ('Software Composition Analysis') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'owasp-dc'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
  
     stage ('Host vulnerability assessment') {
        steps {
             sh 'echo "In-Progress"'
            }
    }
    stage ('Static Application Security Testing') {
	      steps {
		      sh ''''
		      set +e
        	withSonarQubeEnv(credentialsId:'061a7f14-2768-41bf-be18-50ab93096213' , installationName:'SonarQubeScanner') {
	          sh 'mvn sonar:sonar'
		  ''''
				}
	      	}
    	}
    
    }
}
