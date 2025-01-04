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
              sh 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash'
              sh '. ~/.nvm/nvm.sh'
              sh 'nvm install --lts'
              sh 'export PATH="$PATH:$(npm config get prefix)/bin"'
              sh 'npm install -g vercel'
              sh 'vercel --token $VERCEL_TOKEN --confirm --prod'
            }
        }
    }
}
