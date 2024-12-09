pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')  // ID configurado no Jenkins
        AWS_SECRET_ACCESS_KEY = 'o1lF9p2cPkyghIIGiE42eoHoqDYERfpiz6Q/DeZ2' // Secret configurado no Jenkins
        S3_BUCKET = 'projeto-upx06'
        CLOUDFRONT_DIST_ID = 'E2QHBGXQGTL4XZ'
    }
    stages {
        stage('Checkout do Código') {
            steps {
                git branch: 'main', url: 'https://github.com/GiMerguizo/infra-upx06.git'
            }
        }
        stage('Build') {
            steps {
                // Adicione comandos para build (ex.: npm build ou webpack)
                echo 'Build concluído!'
            }
        }
        stage('Upload para S3') {
            steps {
                script {
                    sh '''
                    aws s3 sync . s3://$S3_BUCKET --delete
                    '''
                }
            }
        }
        stage('Invalidar Cache do CloudFront') {
            steps {
                script {
                    sh '''
                    aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DIST_ID --paths "/*"
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Deploy concluído com sucesso!'
        }
        failure {
            echo 'Erro no deploy.'
        }
    }
}
