#!/usr/bin/env groovy
 
pipeline {
  agent any
  stages {
    stage('Deploy') {
      steps {
        notifyBuild('STARTED')

        //Pull the repo first
        checkoutStage()

        sh '/home/vagrant/.config/composer/vendor/bin/phploy --list'
           
      }
    }
  }
  post { 
    always { 
      deleteDir()
    }
    success {
      notifyBuild(currentBuild.result)
    }
  }
}

def notifyBuild(String buildStatus = 'STARTED'){
  buildStatus = buildStatus ?: 'SUCCESSFUL'
}



def checkoutStage() {
    stage('checkout'){
        checkout scm
    }
}

def checkoutTool(branch, repo) {
    checkout([
        $class: 'GitSCM',
        branches: [[name: "*/${branch}"]],
        doGenerateSubmoduleConfigurations: false,
        extensions: [],
        submoduleCfg: [],
        userRemoteConfigs: [[
          credentialsId: 'github',
          url: repo
        ]]
    ])
}