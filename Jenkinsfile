pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }
        stage('Deploy to staging') {
            steps {
                build job: 'Deploy-to-staging'
            }
        }
        stage('Deploy to Production') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message:'Approve PRODUCTION deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}