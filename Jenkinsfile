#!/usr/bin/env groovy
node() {
    stage('Code Checkout'){
        checkout([
          $class: 'GitSCM',
          branches: scm.branches,
          extensions: scm.extensions + [[$class: 'CleanCheckout']] +
            [[$class: 'LocalBranch', localBranch: "${env.BRANCH_NAME}" ]],
          userRemoteConfigs: scm.userRemoteConfigs
        ])
    }
    stage('Build') {
            sayHello( 'Building..' )
    }
    stage('Test') {
            echo 'Testing..'
    }
    stage('Deploy') {
            echo 'Deploying....'
    }
    
    
}
