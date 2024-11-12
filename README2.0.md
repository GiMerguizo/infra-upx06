# Infraestrutura do Projeto da Upx 06

## Ferramentas utilizadas:
- AWS: S3, CloudFront, EC2
- GitHub
- Jenkins
- Grafana
- Prometheus

## Configurações
### AWS
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
        "arn:aws:s3:::SEU_BUCKET/*"
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


## Referências
https://github.com/GiMerguizo/integracao-jenkins-grafana/tree/main