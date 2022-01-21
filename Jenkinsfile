pipeline {
     agent any 

     stages {
          stage('Git Checkout') {
               steps {
                    git branch: 'main', url: 'https://github.com/badcommits/sonar-test.git'
               }
          }

          stage('SonarQube Analysis') {
        
          withCredentials([usernamePassword(credentialsId: 'sonar-creds', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
               withSonarQubeEnv('SonarQube') {
        
               def scannerHome = tool 'Sonar Scanner'
               sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=$USERNAME -Dsonar.password=$PASSWORD";
               sh "sleep 30"
               }
            
               timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                    if (qg.status != 'OK') {
                         error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
               }

          }

          }
     
     }
}