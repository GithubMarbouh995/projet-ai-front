pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
        NPM_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\npm'
    }
    stages {
        stage('Vercel Deploy') {
            steps {
                script {
                    bat """
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Déploiement sur Vercel
                        cd %WORKSPACE%
                        "${NPM_PATH}\\vercel.cmd" deploy --token %VERCEL_TOKEN% --prod > vercel_output.txt
                        if errorlevel 1 (
                            echo Erreur lors du déploiement
                            exit /b 1
                        )

                        echo Contenu du fichier vercel_output.txt:
                        type vercel_output.txt

                        echo Recherche de l'URL de production
                        findstr /C:"Production" vercel_output.txt > url.txt
                        if not exist url.txt (
                            echo URL non trouvée
                            exit /b 1
                        )

                        echo URL trouvée:
                        type url.txt
                    """

                    if (fileExists('url.txt')) {
                        def deployUrl = readFile('url.txt').trim()
                        echo "URL de déploiement: ${deployUrl}"
                        archiveArtifacts artifacts: 'url.txt,vercel_output.txt', allowEmptyArchive: true
                    } else {
                        error "Fichier url.txt non trouvé"
                    }
                }
            }
        }
    }
}
