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
        //nexusUrl='3.90.81.75:8081'
        account_id='533267192490'
        region='us-east-1'
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
               echo "appversion:$appversion"
            """
        } 
       }
       stage('Zipping file'){
       steps{
        sh """
         zip -q -r backend-${appversion}.zip * -x Jenkinsfile -x backend-${appversion}.zip
         ls -lrt
        """
       }
       }

       stage('Docker Build'){
        steps{
            sh """
            aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account_id}.dkr.ecr.${region}.amazonaws.com
            docker build -t ${account_id}.dkr.ecr.${region}.amazonaws.com/expense-backend:${appversion} .
            docker push ${account_id}.dkr.ecr.${region}.amazonaws.com/expense-backend:${appversion}

            """
        }
       }
    /*    stage('nexus-uploader'){
        steps{
            script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appversion}",
                        repository: "backend",
                        credentialsId: 'nexus',
                        artifacts: [
                            [artifactId: "backend",
                            classifier: '',
                            file: "backend-" + "${appversion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
       }
       stage('backendd-deploy'){
        steps{
           script{
                def params= [
                    string(name: 'appVersion', value: "${appversion}")
                ]
                build job: 'backend-deploy', parameters: params, wait:false

           }
        }
       }
    } */
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