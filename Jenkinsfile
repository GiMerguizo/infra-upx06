pipeline {
    agent any
    options {
        skipDefaultCheckout true
        buildDiscarder logRotator(
            artifactDaysToKeepStr: '',
            artifactNumToKeepStr: '',
            daysToKeepStr: '',
            numToKeepStr: '5'
        )
        timeout(
            activity: true,
            time: 30
        )
        disableConcurrentBuilds()
    }/*
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        BUCKET_NAME = 'nome-do-bucket'
    }*/
    stages {/*
        stage('Clonar Código') {
            steps {
                git credentialsId: 'GITHUB_CREDENTIALS_ID', url: 'https://github.com/nome-repo.git'
            }
        }*/
        stage('Build') {
            steps {
                // Exemplo de comando de build, modifique conforme sua aplicação
                echo 'Teste'
                sh 'npm install'
                sh 'npm run build'
            }
        }/*
        stage('Upload to S3') {
            steps {
                script {
                    def s3UploadCommand = """
                        aws s3 sync ./build s3://$BUCKET_NAME --delete
                    """
                    sh "AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID} AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY} ${s3UploadCommand}"
                }
            }
        }*/
    }
}