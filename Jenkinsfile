#!/usr/bin/env groovy
 
pipeline {
  agent any
  stages {
    stage('Deploy') {
      steps {
        notifyBuild('STARTED')

        //Pull the repo first
        checkoutStage()

        //checkoutTool("newfeature", "ssh://git@github.com:aps90/demo.git")
        sh 'ls -la'

        sh 'rm -vfR composer.lock'
        //Run git status to just log anything outstanding
        sh 'git status'

        script{
          switch(env.BRANCH_NAME){
            case "newfeature":
              sh '/home/vagrant/.config/composer/vendor/bin/phploy --init'
              //sh '/home/vagrant/.config/composer/vendor/bin/phploy --list'
              sh '/home/vagrant/.config/composer/vendor/bin/phploy -s newfeature'
              //sh '/home/vagrant/.config/composer/vendor/bin/phploy --list'
              //sh '/home/vagrant/.config/composer/vendor/bin/phploy -s develop --rollback'
            break;
            
            default:
              echo "Skipping deploy, no deploy for branch '${env.BRANCH_NAME}'"
            break;
          }
        }  
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