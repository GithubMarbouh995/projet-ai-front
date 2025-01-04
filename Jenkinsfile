pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Checkout from GitHub') {
            steps {
                deleteDir()
                 git(
                        url: 'https://github.com/GithubMarbouh995/projet-ai-front.git',
                        branch: 'main'
                    )
            }
        }
        stage('Vercel Deploy') {
            steps {
                sh "npm install -g vercel"
                sh "vercel --token $VERCEL_TOKEN --confirm --prod"
            }
        }
    }
}
