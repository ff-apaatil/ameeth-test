#!/usr/bin/env groovy
@Library('ff_library')
import com.ff.SayHello
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
            def t = new SayHello()
            t.sayHello( 'Building..' )
    }
    stage('Test') {
            echo 'Testing..'
    }
    stage('Deploy') {
            echo 'Deploying....'
    }
    
    
}
