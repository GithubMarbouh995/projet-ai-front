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
             bat 'echo %PATH%'
             bat 'for /f "tokens=*" %%i in (\'npm config get prefix\') do set npm_prefix=%%i'
             bat 'setx PATH "%PATH%;%npm_prefix%\\npm"'
             bat "npm install -g vercel"
             bat "vercel --token %VERCEL_TOKEN% --confirm --prod"
            }
        }
    }
}
