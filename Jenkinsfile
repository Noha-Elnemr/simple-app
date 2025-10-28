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
		sh '''
		# Stop any old Node process
		pkill -f "node app.js" || true

		# Start new Node process in background
		nohup node app.js > app.log 2>&1 &
		sleep 3

		# Show if it’s running
		ps aux | grep node
		netstat -tulnp | grep 3000 || true
		'''
	    }
	}

    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}

