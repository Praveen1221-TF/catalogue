pipeline {

// This is pre build section 
    agent {
        node{
            label 'Agent-1'
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = ""
        AccountID = "008616580268"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()  
    }

    // This is build section
    stages {
        stage('Read Version') {
            steps {
                script{
                    
                        def packageJSON = readJSON file: 'package.json'
                        appVersion = packageJSON.version
                        echo " app version is: ${appVersion}"
                    
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script{
                    sh '''
                        npm install 
                    '''
                 }
            }
        }

        stage('Build Image') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-cred') {
                        sh '''
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AccountID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build ${AccountID}.dkr.ecr.us-east-1.amazonaws.com/${Project}/${Component}:${appVersion}
                            docker images
                            docker push ${AccountID}.dkr.ecr.us-east-1.amazonaws.com/${Project}/${Component}:${appVersion}

                        '''
    
                         }
                    }
             }
        }
    }  

    post{
        always{
            echo 'I will always say hello again!'
            cleanWs()
        }
        success{
            echo 'I will run if sucess'

        }
        failure{
            echo 'i will run if fail'

        }
        aborted {
            echo 'pipeline is aborted'
        }
    }
}    