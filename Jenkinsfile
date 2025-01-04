pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token')
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Checkout from GitHub') {
            steps {
                script {
                  //Supprime le repertoire s'il existe deja
                    if (fileExists('projet-ai-front')) {
                        deleteDir()
                    }
                    // Récupérer le code depuis GitHub
                    git(
                        url: 'https://github.com/GithubMarbouh995/projet-ai-front.git', // Remplacez par l'URL de votre dépôt
                        branch: 'main', // Remplacez par votre branche, si nécessaire (ex: develop)
                        // credentialsId: 'github-credentials' // Remplacez par l'ID de vos identifiants GitHub
                    )
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("my-angular-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', $DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Vercel Deploy') {
            steps {
                sh "npm install -g vercel"
                sh "vercel --token $VERCEL_TOKEN --confirm --prod"
            }
        }
    }
}
