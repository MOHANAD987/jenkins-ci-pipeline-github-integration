pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        AWS_REGION = 'us-east-2'
        EKS_CLUSTER = 'eks'
        DOCKER_IMAGE = 'muhnadfisaal/java-jenkins-pipeline'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/MOHANAD987/java-jenkins-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Login to DockerHub & Push') {
            steps {
                script {
                    sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    withAWS(credentials: 'aws-cli', region: "${AWS_REGION}") {
                        sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${EKS_CLUSTER}"
                        sh "kubectl apply -f ./k8s/deployment.yaml"
                    }
                }
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