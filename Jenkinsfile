pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Debug Workspace') {
      steps {
        sh 'echo "Workspace: $WORKSPACE"'
        sh 'ls -la'
      }
    }

    stage('Install Dependencies') {
      steps {
        script {
          // ensure python3 exists
          sh 'python3 --version || true'
          if (fileExists('requirements.txt')) {
            sh 'python3 -m pip install --user -r requirements.txt'
          } else {
            echo 'requirements.txt not found — installing pytest only'
            sh 'python3 -m pip install --user pytest'
          }
        }
      }
    }

    stage('Run Tests') {
      steps {
        sh 'python3 -m pytest -q || true'
      }
    }

    stage('Run Application') {
      steps {
        sh 'python3 app.py'
      }
    }
  }

  post {
    always {
      echo "Cleaning workspace"
      cleanWs()
    }
  }
}
