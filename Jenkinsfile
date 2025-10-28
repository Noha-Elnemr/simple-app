pipeline {
    agent any

    tools {
        nodejs "nodejs18"
    }

    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/Noha-Elnemr/simple-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {
            steps {
                sh 'nohup node app.js > app.log 2>&1 &'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}

