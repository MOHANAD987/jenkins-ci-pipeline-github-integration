pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build in progress'
            }
        }
    }
   post {
    success {
        slackSend(
            channel: '#jenkins-ci',
            color: 'good',
            message: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)",
            teamDomain: 'myteam-oi98964',
            tokenCredentialId: 'slack'
        )
    }
    failure {
        slackSend(
            channel: '#jenkins-ci',
            color: 'danger',
            message: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)",
            teamDomain: 'myteam-oi98964',
            tokenCredentialId: 'slack'
        )
    }
}


}  
