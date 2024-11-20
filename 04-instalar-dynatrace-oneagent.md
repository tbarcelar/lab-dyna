# Instalação do Dynatrace OneAgent com Terraform
Este guia descreve como instalar o Dynatrace OneAgent usando Terraform em um ambiente Ubuntu.


🛠️ Passo a passo

1️⃣ Configuração do arquivo Terraform

1. Crie um arquivo de configuração Terraform chamado dynatrace_oneagent.tf com o conteúdo abaixo:


provider "dynatrace" {
  url = "https://<your-dynatrace-environment>.live.dynatrace.com"
  api_token = "<your-api-token>"
}

resource "dynatrace_oneagent" "example" {
  environment_id = "<your-environment-id>"
}






Substitua os valores <your-dynatrace-environment>, <your-api-token> e <your-environment-id> pelos dados específicos do seu ambiente Dynatrace.



2️⃣ Inicializar o Terraform

terraform init

3️⃣ Aplicar a configuração do Terraform

terraform apply

Obs: Revise as mudanças que o Terraform fará e confirme digitando yes.

4️⃣ Verifique a instalação

sudo systemctl status dynatrace-agent



