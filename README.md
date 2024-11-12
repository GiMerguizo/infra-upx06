# Infraestrutura do Projeto de UPX06 

![infra-site](./Midias/Fluxo%20estrutural%20de%20aplicativos%20web%20AWS.png)

## Ferramentas utilizadas:
- AWS: S3, CloudFront, EC2
- GitHub
- Jenkins
- Grafana
- Prometheus

## Configurações
### AWS
#### S3
- Criar um Bucket para subir os arquivos

#### IAM
- Criação de uma nova política para acesso ao S3. Exemplo:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::NOME_BUCKET/*"
      ]
    }
  ]
}
```
- Criação de um grupo de usuários com a política criada
- Criação de um User
    - Atribuir ao grupo criado
- Entrar nas Configurações do Usuário e criar chaves de acesso (para CLI)

### Jenkins
- Logar no Jenkins
- Instalações de Plugin
  - Git Server
  - AWS S3 Publisher
  - Pipeline 
  - Pipeline Stage View
  - AWS Credentials
- Configurar um agent <br>
:video_camera: https://www.youtube.com/watch?v=99DddJiH7lM
- Criar um novo Job
  - Pipeline Script

```
pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        BUCKET_NAME = 'nome-do-bucket'
    }
    stages {
        stage('Clonar Código') {
            steps {
                git credentialsId: 'GITHUB_CREDENTIALS_ID', url: 'https://github.com/nome-repo.git'
            }
        }
        stage('Build') {
            steps {
                // Exemplo de comando de build, modifique conforme sua aplicação
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Upload to S3') {
            steps {
                script {
                    def s3UploadCommand = """
                        aws s3 sync ./build s3://$BUCKET_NAME --delete
                    """
                    sh "AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID} AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY} ${s3UploadCommand}"
                }
            }
        }
    }
}

```

## Referências
:link: https://github.com/GiMerguizo/integracao-jenkins-grafana/tree/main <br>
:link: https://github.com/cassianobrexbit/dio-live-cloudfront