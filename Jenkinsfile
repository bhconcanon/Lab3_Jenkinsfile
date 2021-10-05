@Library('startendtime') _
pipeline {
  agent any
    libraries {
        lib('startendtime')
    }
    tools{
        maven 'maven 3'
    }
    stages {
        stage ("initialize") {
            steps {
                sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage ('build') {
            steps {
                sh 'mvn clean package'
            }
    }
        stage ('test') {
            steps {
                sh 'mvn test'
            }
    }
        stage ('deploy') {
            steps {
                sh './scripts/deliver.sh'
                }
        }
        stage('Email') {
            steps {
                emailext (
                    subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """SUCCESSFUL: Job '${JOB_NAME} [${BUILD_NUMBER}]':
                    Check console output at ${BUILD_URL}""",
                    to: 'bilal.hussain@concanon.com'
                    )
                }
            }
        stage('StartEndTime') {
            steps {
                buildTime('*')
            }
        }
    }
}  