@Library('my-shared-library@master') _ //master or whatever branch

node {
    
    def buildType = ''
    
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
    
    stage('Sharedlibrary test') {
        //filterLogs('checkout', 1)
        gradleBuild()
    }
    /*
    stage('Gradle Static Analysis'){
        withSonarQubeEnv('mysonarserver') {
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
        
    }
    */
    /*
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
    }*/
    /*
    stage('Nexus Push') {
        nexusArtifactUploader artifacts: [
            [
                artifactId: 'HelloWorld', classifier: '', file: 'build/libs/HelloWorld.jar', type: 'jar'
            ]
        ], 
            credentialsId: 'nexususer', 
            groupId: 'com.shiva.test', 
            nexusUrl: '10.128.0.23:8081/', 
            nexusVersion: 'nexus3', 
            protocol: 'http', 
            repository: 'shiva-release', 
            version: "${buildVersionNumber}"
    }
    
    stage('copy nexus data') {
        sh "echo sudo wget http://104.198.202.130:8081/repository/shiva-release/com/shiva/test/HelloWorld/'${buildVersionNumber}'/HelloWorld-'${buildVersionNumber}'.jar >> ./user_data_file"
        sh "echo sudo cp HelloWorld-'${buildVersionNumber}'.jar /opt/tomcat/latest/webapps/ >> ./user_data_file"
    }*/
    /*
    stage('create launch configuration') {
        infraNumber = env.BUILD_NUMBER + '-' + SHORT_GIT_COMMIT
        sh "/usr/bin/ansible-playbook launch_config.yml --extra-vars=\"launch_config=launchconfig-${infraNumber} elb_name=loadbalancer-${infraNumber} asg_name=autoscale-${infraNumber}\"  --vault-password-file=\"/home/rajgali83/pass.txt\""
    }
    */
    /*
    stage('Ansible deploy') {
        try {
            if("${buildType}" == "-RELEASE"){
                repo_name="sample-repo"
                unix_dest="/stage/"
                mc_cred="fldrwds"
                file_name = "${unix_grp}" + '-' + "${CompName}" + '-' + "${buildVersionNumber}"
                echo "release file name is ${file_name}"
            } else {
                repo_name="snapshot-repo"
                unix_dest="/testlocation/"
                mc_cred="fldrwcdt"
                date=new Date().format("yyyyMMdd")
                search_filename = "${unix_grp}" + '-' + "${CompName}" + '-' + "${buildVersionNumber}"
                echo "find file name is ${search_filename}"
                
                //sh "grep ${}"
                echo "snopshot file name is ${search_filename}"
            }
            
            artifact_id = "${unix_grp}" + '-' + "${CompName}"
            group_id = "com/shiva/test/helloworld"
            unix_mode="0540"
            file_extension="jar"
            unix_grp="${unix_grp}"
            date_time=new Date().format("yyyyMMdd'_'HHmmss")
            echo "Deploying file : ${file_name} from ${repo_name} to ${unix_dest}"
            
            //check ansible installed on jenkins/ansible server
            sh "tower-cli job launch --job-template=JarDeploy --credential=${mc_cred} --extra-vars=\"repo_name=${repo_name}""
            //sh "ansible-playbook"
        } catch (e) {
            currentBuild.result = "DEPLOY FAILED"
            throw e
        } finally {
            echo "finally block - ansible deploy"
            //send notification for - build results to mail and slack
        }
    }
    */
    
}    

def notifySonarStatus() {
    echo "Notify block"
    /*
    def buildStatus = currentBuild.currentResult    
    emailext (
        subject: "'${buildStatus}': Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
        body: """<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",
        mimeType: "text/html",
        to: "shivakumargali83@gmail.com",
        from: "shivakumar399@gmail.com"
    )
    */
}

