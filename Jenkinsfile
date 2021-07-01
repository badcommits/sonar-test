node(docker){

    stage('SCM Checkout') {
        git credentialsId: 'github', url: 'https://github.com/badcommits/sonar-test.git'
    }

    stage('SonarQube Analysis') {
        
    withSonarQubeEnv('My SonarQube Server') {
      sh 'mvn clean package sonar:sonar'
    }
        
    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
        if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }

    }

}