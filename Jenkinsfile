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
              sh """
                curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
                . ~/.nvm/nvm.sh
                nvm install --lts
                export PATH="\$PATH:\$(npm config get prefix)/bin"
                npm install -g vercel
                vercel --token \$VERCEL_TOKEN --confirm --prod
              """
            }
        }
    }
}
