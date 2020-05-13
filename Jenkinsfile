node {
    stage('Checkout') {
        checkout scm
    }
    /*
    stage('Gradle Static Analysis'){
        withSonarQubeEnv('shivasonarqube') {
            sh "./gradlew clean sonarqube"
        }
    }
    
    stage('Quality Gates'){
        try {
            timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        } catch (Exception ex) {
            notifySonarStatus()
        } finally {
            notifySonarStatus()
        }
        
    }*/
}    
/*
def notifySonarStatus() {
    def buildStatus = currentBuild.currentResult
    
    
    emailext (
        subject: "'${buildStatus}': Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
        body: """<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",
        mimeType: "text/html",
        to: "shivakumargali83@gmail.com",
        from: "shivakumar399@gmail.com"
    )
}
*/
