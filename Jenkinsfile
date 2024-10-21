pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                  - name: maven
                    image: maven:3.8.4-openjdk-17
                    command:
                    - cat
                    tty: true
            '''
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }

    environment {
        APP_NAME = "DCUBE_APP"
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

        stage ("Checkout from SCM") {
                   steps{
                    git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/Musyoka534/jenkins-pipeline.git'
                   }

           }

        stage('Code Build') {
            steps {
                container('maven') {
                 sh 'mvn install -Dmaven.test.skip=true'
                }
            }
        }

        stage('Printing All Global Variables') {
            steps {
                sh """
                env
                """
            }
        }

    }   
}
