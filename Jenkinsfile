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

                        echo Vérification de l'installation
                        "${NPM_PATH}\\vercel.cmd" -v || exit 1

                        echo Déploiement sur Vercel
                        cd %WORKSPACE%
                        "${NPM_PATH}\\vercel.cmd" --token %VERCEL_TOKEN% --confirm --prod > vercel_output.txt 2>&1
                        if %ERRORLEVEL% NEQ 0 (
                            echo Erreur de déploiement
                            type vercel_output.txt
                            exit 1
                        )

                        echo Extraction URL
                        findstr /C:"Production:" vercel_output.txt > deployment_url.txt
                        if not exist deployment_url.txt (
                            echo URL non trouvée
                            exit 1
                        )

                        echo URL du déploiement:
                        type deployment_url.txt
                    """

                    archiveArtifacts artifacts: 'deployment_url.txt,vercel_output.txt', allowEmptyArchive: true
                }
            }
            post {
                always {
                    script {
                        if (fileExists('deployment_url.txt')) {
                            echo "URL de déploiement:"
                            bat 'type deployment_url.txt'
                        }
                    }
                }
            }
        }
    }
}
