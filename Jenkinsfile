pipeline {
  agent any

  environment {
    NODEJS_HOME = tool name: 'nodejs18', type: 'NodeJSInstallation'
    PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
    APP_PORT = '3000'
    PM2_APP_NAME = 'simple-app'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'node -v && npm -v'
        sh 'npm ci || npm install'
      }
    }

    stage('Start/Restart app with PM2') {
      steps {
        sh '''
          if ! command -v pm2 > /dev/null 2>&1; then
            npm install -g pm2
          fi
          # make sure PM2 uses the Jenkins workspace
          pm2 describe ${PM2_APP_NAME} >/dev/null 2>&1 && pm2 delete ${PM2_APP_NAME} || true
          # start app with env PORT
          pm2 start app.js --name ${PM2_APP_NAME} --env production -- -p ${APP_PORT}
          pm2 save
          pm2 ls
        '''
      }
    }
  }

  post {
    always {
      sh 'pm2 status || true'
    }
  }
}

