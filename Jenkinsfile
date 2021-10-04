#!groovy

// clone repo
  stages {
    stage('Checkout SCM') {
      steps{
        mail to: 'devi.com'
              body: "Building '${JOB_NAME}' - '${BUILD_DISPLAY_NAME}' @ '${NODE_NAME}'.\nYou would receive an email log after this job is finished.\n-Talos", 
              cc: 'devi.com',
              from: 'devi.com' 
              replyTo: 'devi.com',
              subject: "Build Started - '${JOB_NAME}'"
              

        git branch: 'master', changelog: false, credentialsId: 'devigithub_Config', url: "gitrepourl"

        script{
          env.GIT_TAG_NAME = gitTagName()
          // env.GIT_TAG_MESSAGE = gitTagMessage()
        }
      }
    }
    // kubernetes deployment
    stage('Deploy to k8s')
    {
      steps{        
        sh '''
        sed -i "s/pbsalgo:latest/pbsalgo:${GIT_TAG_NAME}/g" mediawiki.yaml.yaml
        '''
        kubernetesDeploy configs: 'mediawiki.yaml', kubeConfig: [path: ''], kubeconfigId: 'minikube', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://192.168.49.2:8443']
      }
    }

  }
  
  // send result
  post{
    always
    {
      echo 'Completed'
      emailext attachLog: true, body: '$DEFAULT_CONTENT',
                postsendScript: '$DEFAULT_POSTSEND_SCRIPT',
                presendScript: '$DEFAULT_PRESEND_SCRIPT',
                replyTo: '$DEFAULT_REPLYTO',
                subject: '$DEFAULT_SUBJECT',
                from: 'devi.com',
                to: 'devi.com, cc:devi.com',
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']]
    }
  }
  
}

/** @return The tag name, or `null` if the current commit isn't a tag. */
/** use commitID if no tags present */
String gitTagName() {
    commit = getCommit()
    if (commit) {
        desc = sh(script: "git describe --tags ${commit}", returnStdout: true)?.trim()
        if (isTag(desc)) {
            return desc
        }
    }
    // return null
    return commit
}

/** @return The tag message, or `null` if the current commit isn't a tag. */
String gitTagMessage() {
    name = gitTagName()
    msg = sh(script: "git tag -n10000 -l ${name}", returnStdout: true)?.trim()
    if (msg) {
        return msg.substring(name.size()+1, msg.size())
    }
    return null
}

String getCommit() {
    return sh(script: 'git rev-parse HEAD', returnStdout: true)?.trim()
}

@NonCPS
boolean isTag(String desc) {
    match = desc =~ /.+-[0-9]+-g[0-9A-Fa-f]{6,}$/
    result = !match
    match = null // prevent serialisation
    return result
}