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
                        "${NPM_PATH}\\vercel.cmd" deploy --token %VERCEL_TOKEN% --prod > vercel_deploy.log

                        echo Capture de l'URL
                        findstr "Production URL" vercel_deploy.log > url.txt

                        echo URL de déploiement:
                        type url.txt

                        echo Sauvegarde des fichiers
                        copy url.txt deployment_url.txt
                        copy vercel_deploy.log vercel_output.txt
                    """

                    def deployUrl = readFile('url.txt').trim()
                    echo "URL de déploiement: ${deployUrl}"

                    archiveArtifacts artifacts: '**/url.txt,**/vercel_deploy.log', allowEmptyArchive: true
                }
            }
        }
    }
    post {
        success {
            echo '=== URL de Production ==='
            bat 'type url.txt'
        }
    }
}
