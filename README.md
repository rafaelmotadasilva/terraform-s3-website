<h1>
    <img align="center" width="40px" src="./terraform.svg" alt="Terraform logo">
    <span>Criando um bucket S3 com Terraform</span>
</h1>

Repositório desenvolvido para fins educativos.

## Objetivo
Criar um bucket no S3 com um site estático (utilizando o Webservice ViaCEP) por meio do Terraform.

## Estrutura do projeto

Certifique-se de que o projeto tenha a seguinte estrutura:

```
.
├── index.html
├── main.tf
└── variables.tf
```

## Exemplo de Código Terraform

Aqui está um exemplo de código Terraform que realiza as seguintes tarefas:

- Criar um bucket S3: O bucket será utilizado para hospedar o site estático.

- Criar um objeto S3: O arquivo index.html será armazenado no bucket.

- Configuração do Website no S3: Configurar o bucket para servir conteúdo como um website estático.

- Política de Acesso ao Bucket: Permitir acesso público ao conteúdo do bucket.

`main.tf`

```
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.64.0"
    }
  }
}

provider "aws" {
  region     = "us-east-1"
  access_key = var.access_key
  secret_key = var.secret_key
}

resource "aws_s3_bucket" "example" {
  bucket = "My Bucket"
}

resource "aws_s3_object" "object" {
  bucket = aws_s3_bucket.example.bucket
  key    = "index.html"
  source = "index.html"
  content_type = "text/html"
}

resource "aws_s3_bucket_website_configuration" "example" {
  bucket = aws_s3_bucket.example.bucket

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }
}

resource "aws_s3_bucket_public_access_block" "example" {
  bucket = aws_s3_bucket.example.bucket

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_policy" "example" {
  bucket = aws_s3_bucket.example.bucket

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::examplo/*"
    }
  ]
}
POLICY
}
```

`variables.tf`

```
variable "access_key" {
  description = "my-access-key"
  type        = string
}

variable "secret_key" {
  description = "my-secret-key"
  type        = string
}
```

## Instruções

No diretório onde estão os arquivos `.tf`, execute:

```
terraform init
terraform apply
```