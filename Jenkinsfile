pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build Docker image') {
      steps {
        sh 'docker build -t sample-node-app:${BUILD_NUMBER} .'
      }
    }

    stage('Run Docker container (for quick smoke)') {
      steps {
        sh '''
          docker rm -f sample-node-app || true
          docker run -d --name sample-node-app -p 3000:3000 sample-node-app:${BUILD_NUMBER}
        '''
      }
    }
  }
}
