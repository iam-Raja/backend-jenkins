pipeline {
    agent {
        label 'agent-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment{
        def appversion=''
    }
    
    // stages {
    //     stage('reading version'){
    //         steps{
    //             script{
    //                 def package = readJSON file: 'package.json'
    //                 appversion = package.version
    //                 echo "appversion:$appversion"
    //             }
    //         }
    //     }
        stage('install dependices')
        steps{
            sh """
               npm install
               ls -lrt
               
            """

        }
       
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}