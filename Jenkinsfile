pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/fcong922/internal.git'
            }
        }
        stage('stage 2') {
            environment {
                PORT = 8081
            }
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('stage 3') {
            steps {
                sh 'gcloud builds submit --tag gcr.io/sylvan-terra-309902/internal-image:v${BUILD_NUMBER} .'
            }
        }

        stage('stage 4') {
            steps {
                sh 'gcloud container clusters get-credentials devops-cluster --zone us-east4-c --project sylvan-terra-309902'
                sh 'kubectl set image deployment/events-internal internal-image=gcr.io/sylvan-terra-309902/internal-image:v${BUILD_NUMBER} --record -n development'
            }
        }
    }

}
