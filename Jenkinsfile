pipeline {

    agent {
        node {
            label 'master'
        }
    }

    tools { 
        maven 'maven3' 
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }

    environment {
        APP_NAME = "STUDENT_APP"
        APP_ENV  = "DEV"
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace for ${APP_NAME}"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                ])
            }
        }

        stage('Code Build') {
            steps {
                 /*sh 'mvn install -Dmaven.test.skip=true'*/
                sh """
                echo "Code Build"
                """
            }
        }

        
        stage('Environment Analysis') {

            parallel {

                stage('Priting All Global Variables') {
                    steps {
                        sh """
                        env
                        """
                    }
                }

                stage('Execute Shell') {
                    parallel {
                       stage('Execute Shell 1') {
                         steps {
                           sh 'echo "Hello Parallel 1. Thanks for keeping up!"'
                         }
                       stage('Execute Shell 2') {
                         steps {
                           sh 'echo "Hello Parallel 2. Thanks for keeping up!"'
                         }
                       stage('Execute Shell 3') {
                         steps {
                           sh 'echo "Hello Parallel 3. Thanks for keeping up!"'
                         }    
                       }
                    }
                }

                stage('Print ENV variable') {
                    steps {
                        sh "echo ${APP_ENV}"
                    }
                }
            
            }
        }
    }   
}
