pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_6'  
    }
   stages{
        stage('CompileandRunSonarAnalysis') {
            steps {
        sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=jenkins-sonar-ci_pipeline -Dsonar.organization=jenkins-sonar-ci -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6b97f056496f86ba20857ab4349e4ae20fd980a1'        	
		  }     
        }
        stage('Publish to Artifactory') {
            steps {
                script {
                    // Configure Artifactory details
                    def server = Artifactory.server('artifactory')
                    def buildInfo = Artifactory.newBuildInfo()
                    // Define the Artifactory upload specification 
                    def uploadSpec = """
                        {
                            "files": [
                                {
                                    "pattern": "target/easybuggy.jar",
                                    "target": "default-generic-local"                              }
                            ]
                        }
                    """
                

                    // Upload Artifact to Artifactory
                    server.upload(uploadSpec, buildInfo)
                    // Add build information
                    buildInfo.name = 'easybuggy'
                    buildInfo.number = '2'

                    // Publish build information to Artifactory
                    server.publishBuildInfo(buildInfo)                    
                }
            }
        }
        stage('Xray Scan') {
            steps {
                script {
                    def server = Artifactory.server('artifactory')
                    
                    def scanConfig = [
                        buildName: 'easybuggy',
                        buildNumber: '2',
                        failBuild: true
                    ]
                    
                    def scanResults = server.xrayScan(scanConfig)
                    

                }
            }
        }  
  }
}
