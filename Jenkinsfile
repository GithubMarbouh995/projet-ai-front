pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
        NODE_PATH = 'C:\\Program Files\\nodejs'
        NPM_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\npm'
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

        stage('Prepare NPM Environment') {
            steps {
                script {
                    bat '''
                        if not exist "C:\\ProgramData\\Jenkins\\.jenkins\\npm" mkdir "C:\\ProgramData\\Jenkins\\.jenkins\\npm"
                        npm config set prefix "C:\\ProgramData\\Jenkins\\.jenkins\\npm"
                        set PATH=%NODE_PATH%;C:\\ProgramData\\Jenkins\\.jenkins\\npm;%PATH%
                    '''
                }
            }
        }

        stage('Vercel Deploy') {
            steps {
                script {
                    bat '''
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Vérification de l'installation
                        "C:\\ProgramData\\Jenkins\\.jenkins\\npm\\vercel.cmd" -v

                        echo Déploiement
                        "C:\\ProgramData\\Jenkins\\.jenkins\\npm\\vercel.cmd" --token %VERCEL_TOKEN% --confirm --prod
                    '''
                }
            }
        }
    }
}
