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
                    // Installation et déploiement Vercel
                    bat """
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Vérification de l'installation
                        "${NPM_PATH}\\vercel.cmd" --version

                        echo Déploiement
                        "${NPM_PATH}\\vercel.cmd" --token %VERCEL_TOKEN% --confirm --prod > vercel_output.txt 2>&1

                        echo Création du fichier deployment_url
                        type vercel_output.txt | findstr "Production" > deployment_url.txt

                        echo Contenu du fichier deployment_url.txt:
                        type deployment_url.txt
                    """

                    // Archivage des artifacts
                    archiveArtifacts artifacts: '**/*_url.txt', allowEmptyArchive: true
                }
            }
        }
    }
}
