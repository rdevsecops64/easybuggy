pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mkdir -p sonar'    
		sh 'mvn clean verify -l sonar/log.txt sonar:sonar -Dsonar.projectKey=jenkins-sonar-ci_pipeline -Dsonar.organization=jenkins-sonar-ci -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6b97f056496f86ba20857ab4349e4ae20fd980a1'
		}
            post {
               always {
               archiveArtifacts artifacts: 'sonar/log.txt'
             }
         }
      } 

    stage('Publish SonarCloud Logs to Artifactory') {
            steps {
                script {
                    // Configure Artifactory details
                    def server = Artifactory.server('artifactory')

                    // Define the Artifactory upload specification for SonarCloud logs
                    def uploadSpec = """
                        {
                            "files": [
                                {
                                    "pattern": "sonar/log.txt",
                                    "target": "https://rdevsecops64.jfrog.io/artifactory/1264/"
                                }
                            ]
                        }
                    """

                    // Upload SonarCloud logs to Artifactory
                    server.upload(uploadSpec)
                }
            }
        }
  }
}
