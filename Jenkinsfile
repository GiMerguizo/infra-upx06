pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')  // ID configurado no Jenkins
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key') // Secret configurado no Jenkins
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
        stage('Testar AWS CLI') {
            steps {
                sh 'aws --version'
            }
        }

        stage('Upload para S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-access-key', // ID configurado no Jenkins
                ]]) {
                    sh '''
                    aws s3 sync . s3://projeto-upx06 --delete
                    '''
                }
            }
        }

        stage('Invalidar Cache do CloudFront') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-access-key', // Substitua pelo ID configurado
                ]]) {
                    sh '''
                    aws cloudfront create-invalidation --distribution-id E2QHBGXQGTL4XZ --paths /*
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
