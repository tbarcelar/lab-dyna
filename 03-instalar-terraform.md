# Instalação do Dynatrace OneAgent com Terraform
Este guia descreve como instalar o Dynatrace OneAgent usando Terraform em um ambiente Ubuntu.


🛠️ Passo a passo

1. Atualize os pacotes do sistema:
sudo apt update && sudo apt upgrade -y


2. Instale os pacotes necessários:
sudo apt install -y software-properties-common gnupg

3. Adicione a chave GPG do repositório oficial do Terraform:
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

4. Adicione o repositório oficial do Terraform à lista de fontes do APT:
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list


----------------------- Instalar o Terraform -------------------------

1. Atualize novamente a lista de pacotes:
sudo apt update


2. Instale o Terraform:
sudo apt install -y terraform

3. Verifique se o Terraform foi instalado corretamente
terraform --version




