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

        stage('Deploy') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }    

             when { 
                expression { "$params.DEPLOY" == "true" }

            }
            steps {
                script{
                    sh '''
                        echo "Deploying"

                    '''
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