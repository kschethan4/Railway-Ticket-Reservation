
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading Railway Reservation source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building application'

                bat '''
                if not exist index.html exit /b 1
                if not exist style.css exit /b 1
                if not exist script.js exit /b 1
                echo Build successful
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application'

                bat '''
                findstr /i "<html" index.html
                if errorlevel 1 exit /b 1
                echo Test successful
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Creating ZIP package'

                bat '''
                powershell -NoProfile -Command "Compress-Archive -Path index.html,style.css,script.js -DestinationPath railway-reservation.zip -Force"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
            archiveArtifacts artifacts: 'railway-reservation.zip',
                 fingerprint: true
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
