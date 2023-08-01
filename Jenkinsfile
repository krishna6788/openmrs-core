pipeline {
    agent { label 'Openmrs'}
    trigger {
        pollSCM ('* * * * *')
    }
    parameters {
        string(name: 'Maven_goal', defaultValue: 'package', description: 'This is the parameter for maven' )
    }
    stages {
        stage('Version Control System') {
            steps {
                git url: 'https://github.com/krishna6788/openmrs-core.git',
                    branch: 'Declarative'
            }
        }
        stage('Changeing The Java Path') {
            steps {
                sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH"'
            }
        }
        stage('Build Stage') {
            steps {
                sh "mvn ${params.Maven_goal}"
            }
        }
        stage('Post Build') {
            steps {
                archiveArtifacts artifacts: '**/openmrs.war',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true
                junit testResults: 'target/surefire-reports/TEST-*.xml'
            }
        }
    }
}