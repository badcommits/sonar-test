def label = "docker"

node(label){

    def GIT_BRANCH = "main"
    def GIT_CREDS = "github"
    def GIT_REPO = "https://github.com/badcommits/sonar-test.git"

    stage('SCM Checkout') {
        git branch: GIT_BRANCH, credentialsId: GIT_CREDS, url: GIT_REPO
    }

    stage('SonarQube Analysis') {
        
    withCredentials([usernamePassword(credentialsId: 'sonar-login', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){

        withSonarQubeEnv('My SonarQube Server') {
        
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