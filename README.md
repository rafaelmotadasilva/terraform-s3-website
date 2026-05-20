# Site Estático no S3 com Terraform

Provisionamento de um bucket S3 configurado para hospedar site estático via Terraform — infraestrutura como código do início ao deploy.

## Stack

- **Terraform** — provisionamento da infraestrutura na AWS
- **Amazon S3** — armazenamento e hospedagem do site estático
- **HTML** — página estática de exemplo integrando a API ViaCEP

## Estrutura

```
.
├── main.tf          # Recursos AWS: bucket S3, objeto e configuração de website
├── variables.tf     # Variáveis do projeto (nome do bucket, região)
└── index.html       # Página estática com consulta à API ViaCEP
```

## O que o Terraform provisiona

- **Bucket S3** com nome configurável via variável
- **Objeto S3** (`index.html`) carregado automaticamente
- **Website hosting** habilitado no bucket com documento de índice definido
- **Bucket policy** para acesso público de leitura

## Pré-requisitos

- [Terraform](https://developer.hashicorp.com/terraform/downloads) instalado
- AWS CLI configurado com credenciais válidas (`aws configure`)
- Permissões IAM para criar e configurar buckets S3

## Como usar

```bash
git clone https://github.com/rafaelmotadasilva/terraform-s3-website.git
cd terraform-s3-website

terraform init
terraform plan
terraform apply
```

Após o apply, o Terraform exibe a URL pública do site.

## Variáveis

| Variável | Descrição | Padrão |
|---|---|---|
| `bucket_name` | Nome do bucket S3 | — |
| `aws_region` | Região AWS | `us-east-1` |

## Destruir a infraestrutura

```bash
terraform destroy
```

## Referências

- [Documentação do Terraform — AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [S3 Static Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [API ViaCEP](https://viacep.com.br/)
