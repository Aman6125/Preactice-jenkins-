pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
                echo "Workspace cleared"
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Installing dependencies (if requirements.txt exists)"
                sh 'python3 -m pip install --user -r requirements.txt || true'

                echo "Building (compile python files to check syntax)..."
                sh 'python3 -m compileall .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests (pytest)"
                sh 'python3 -m pytest -q'
            }
        }

        stage('Deploy (demo only)') {
            steps {
                echo "Simulating deployment for demo (no real cloud deploy)"
                sh '''
                  rm -rf deploy_preview || true
                  mkdir -p deploy_preview
                  cp -v app.py README.md requirements.txt deploy_preview/ || true
                  cp -rv tests deploy_preview/ || true
                  tar -czf deploy_preview.tar.gz deploy_preview
                '''
                archiveArtifacts artifacts: 'deploy_preview.tar.gz', fingerprint: true
                echo "Deploy simulation ready — artifact archived as deploy_preview.tar.gz"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed SUCCESSFULLY."
        }
        failure {
            echo "Pipeline FAILED. Check console output for errors."
        }
        always {
            echo "End of pipeline."
        }
    }
}
