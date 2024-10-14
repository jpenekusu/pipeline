pipeline {
    
    agent any

    tools {
        maven "M3"
    }
    triggers {
        cron('H */8 * * *') //regular builds
        pollSCM('* * * * *') //polling for changes, here once a minute
    }

    stages {
        stage('Checkout') {
            steps { //Checking out the repo
                checkout changelog: true, poll: true, scm: [$class: 'GitSCM', branches: [[name: '*/master']], 
                extensions: scm.extensions, userRemoteConfigs: [[credentialsId: 'github_token', url: 'https://github.com/jpenekusu/pipeline.git']]]
                sh "ls -lart ./"    
            }
        }
        stage('Unit & Integration Tests') {
            steps {
                sh "mvn clean package"
            }
        }
    }
    post {
        always { //Send an email to the person that broke the build
            sh "echo 'in Always'"
        }
    }
}