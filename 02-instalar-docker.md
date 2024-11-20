# Instalação do Docker no Ubuntu

Este guia descreve como instalar o Docker em um ambiente Ubuntu.

🛠️ Passo a passo
1️⃣ Atualizar pacotes do sistema
sudo apt update && sudo apt upgrade -y

2️⃣ Instalar dependências necessárias
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

3️⃣ Adicionar chave GPG do repositório oficial do Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

4️⃣ Adicionar o repositório Docker à lista de fontes
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

5️⃣ Instalar o Docker
sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io -y


6️⃣ Verifique a versão do Docker instalada:
docker --version

7️⃣ (Opcional) Usar o Docker sem sudo
sudo usermod -aG docker $USER


