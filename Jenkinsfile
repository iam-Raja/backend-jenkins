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
        def appversion= ''
    }
    
    stages {
        stage('reading version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appversion = packageJson.version
                    echo "appversion : $appversion"
                }
            }
        }
        stage('install dependices'){
        steps{
            sh """
               npm install
               ls -lrt
               echo "appversion:$appversion"
            """

        }
       
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