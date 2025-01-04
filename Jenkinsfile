pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
        NODE_PATH = 'C:\\Program Files\\nodejs'
        NPM_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\npm'
    }
    stages {
        // ...existing code...
        stage('Vercel Deploy') {
            steps {
                script {
                    bat '''
                        echo Installation de Vercel
                        npm install -g vercel

                        echo Déploiement
                        "C:\\ProgramData\\Jenkins\\.jenkins\\npm\\vercel.cmd" --token %VERCEL_TOKEN% --confirm --prod > deployment_url.txt

                        echo URL du déploiement:
                        type deployment_url.txt
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
