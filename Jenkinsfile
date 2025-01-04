pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
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
        stage('Vercel Deploy') {
            steps {
                script {
                    bat '''
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Déploiement sur Vercel
                        cd %WORKSPACE%
                        "%NPM_PATH%\\vercel.cmd" deploy --token %VERCEL_TOKEN% --prod > vercel_deploy.log 2>&1

                        echo Extraction URL de déploiement
                        findstr /C:"Preview:" vercel_deploy.log > url.txt
                        findstr /C:"Production:" vercel_deploy.log >> url.txt

                        echo URL de déploiement:
                        type url.txt
                    '''

                    // Archivage des URLs
                    archiveArtifacts artifacts: 'url.txt,vercel_deploy.log', allowEmptyArchive: true
                }
            }
            post {
                success {
                    echo 'URL de déploiement :'
                    bat 'type url.txt'
                }
            }
        }
    }
}
