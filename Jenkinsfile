pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
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
               sh 'sudo apt update'
               sh 'sudo apt install -y nodejs npm'
               sh 'echo $PATH'
               sh 'export PATH="$PATH:$(npm config get prefix)/bin"'
                sh "npm install -g vercel"
                sh "vercel --token $VERCEL_TOKEN --confirm --prod"
            }
        }
    }
}
