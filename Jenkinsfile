pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=jenkins-sonar-ci_pipeline -Dsonar.organization=jenkins-sonar-ci -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6b97f056496f86ba20857ab4349e4ae20fd980a1'
			}
        } 

        
    stage ('Upload file') {
            steps {
                rtUpload (
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
                    serverId: artifactory,
                    spec: """{
                            "files": [
                                    {
                                       "target": "libs-snapshot-local"
                                    }
                                ]
                            }"""
                )
            }
        }
    }
}
