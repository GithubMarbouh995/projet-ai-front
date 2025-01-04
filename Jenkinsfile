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
        stage('Vercel Deploy') {
            steps {
                script {
                    bat '''
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Déploiement
                        vercel --token %VERCEL_TOKEN% --confirm --prod > deployment_url.txt

                        echo URL du déploiement:
                        type deployment_url.txt
                    '''

                    // Vérifiez si le fichier deployment_url.txt existe
                    bat '''
                        if exist deployment_url.txt (
                            echo Le fichier deployment_url.txt a été créé avec succès.
                        ) else (
                            echo Le fichier deployment_url.txt n'a pas été créé.
                            exit 1
                        )
                    '''

                    // Archiver l'URL du déploiement
                    archiveArtifacts artifacts: 'deployment_url.txt', allowEmptyArchive: true
                }
            }
        }
    }
    post {
        success {
            echo 'Application déployée avec succès! Accédez à votre application sur:'
            bat 'type deployment_url.txt'
        }
    }
}
