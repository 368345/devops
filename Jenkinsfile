pipeline {
    agent none
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dh_cred') // This uses the ID of your stored credentials
    }
    stages {
        stage('Checkout'){
            agent any
            steps{
                checkout scm
            }
        }

        stage('Init'){
            agent any
            steps {
                bat 'docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%
            }
        }

        stage('Build Backend'){
            agent any
            when {
                changeset "**/backend/**/*.*"
                beforeAgent true
            }
            steps {
                dir('backend'){
                    bat 'docker build -t %DOCKERHUB_CREDENTIALS_USR%/gestion_backend:%BUILD_ID% .'
                    bat 'docker push %DOCKERHUB_CREDENTIALS_USR%/gestion_backend:%BUILD_ID%'
                    bat 'docker rmi %DOCKERHUB_CREDENTIALS_USR%/gestion_backend:%BUILD_ID%'
                }
            }
        }

        stage('Build Frontend'){
            agent any
            when {
                changeset "**/frontend/**/*.*"
                beforeAgent true
            }
            steps {
                dir('frontend'){
                    bat 'docker build -t %DOCKERHUB_CREDENTIALS_USR%/gestion_frontend:%BUILD_ID% .'
                    bat 'docker push %DOCKERHUB_CREDENTIALS_USR%/gestion_frontend:%BUILD_ID%'
                    bat 'docker rmi %DOCKERHUB_CREDENTIALS_USR%/gestion_frontend:%BUILD_ID%'
                }
            }
        }

        stage('Logout'){
            agent any
            steps {
                bat 'docker logout'
            }
        }

    }
}
