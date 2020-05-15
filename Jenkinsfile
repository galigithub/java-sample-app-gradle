node {
    stage('Checkout') {
        echo "Branch - ${env.BRANCH_NAME}"
        echo "Job name - ${env.JOB_NAME}"
        
        def branch = env.BRANCH_NAME
        def job = env.JOB_NAME
        
        if(branch != null) {
            if("${branch}" =~ /^master$/) {
                buildType = '-RELEASE'
            } else {
                buildType = '-SNAPSHOT'
            }
        }
        echo "buildType - ${buildType}"
        
        checkout scm
    }
    
    stage('Git Commit Hash') {
        sh 'git rev-parse HEAD > commit'
        git_commit = readFile('commit').trim()
        SHORT_GIT_COMMIT = git_commit.take(7)
        echo "Short git commit = ${SHORT_GIT_COMMIT}"
        buildVersionNumber = env.BUILD_NUMBER + '-' + SHORT_GIT_COMMIT + buildType
        echo "file_name ${buildVersionNumber}"
    }
    
    stage('Gradle Build') {
        try {
            echo 'Build Started'
            currentBuild.displayName = "${env.BUILD_NUMBER + '-' + SHORT_GIT_COMMIT}"
            buildVersionNumber = env.BUILD_NUMBER + '-' + SHORT_GIT_COMMIT + buildType
            sh 'chmod +x ./gradlew'
            sh './gradlew clean build '
            archiveArtifacts 'build/libs/*'
        } catch (e) {
            currentBuild.result = "FAILED"
            throw e
        } finally {
        }
    }
    
    stage('Nexus Push') {
        nexusArtifactUploader artifacts: [
            [
                artifactId: 'HelloWorld', classifier: '', file: 'build/libs/HelloWorld.jar', type: 'jar'
            ]
        ], 
            credentialsId: 'nexusadmin', 
            groupId: 'com.shiva.test', 
            nexusUrl: '35.226.116.179:8081/', 
            nexusVersion: 'nexus3', 
            protocol: 'http', 
            repository: 'sample-repo', 
            version: '1.0.0'
    }
    
    stage('Ansible deploy') {
        try {
            if("${buildType}" == "-RELEASE"){
                repo_name="sample-repo"
                unix_dest="/stage/"
            }
        }
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
