<h1>
Instalação Terraform
</h1>

## 🛠️ Passo a passo

### 1️⃣ Atualizar pacotes do sistema

- Atualize os pacotes do sistema:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

##

### 2️⃣ Instalar pacotes necessários

- Instale os pacotes requisitados:
    ```bash
    sudo apt install -y software-properties-common gnupg
    ```

##

### 3️⃣ Adicionar a chave GPG do repositório oficial do Terraform

- Configure a chave GPG do repositório oficial do Terraform:
    ```bash
    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    ```

##

### 4️⃣ Adicionar o repositório oficial do Terraform à lista de fontes

- Adicione o repositório ao APT:
    ```bash
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    ```

##

### 5️⃣ Instalar o Terraform

- Atualize a lista de pacotes:
    ```bash
    sudo apt update
    ```

- Instale o Terraform:
    ```bash
    sudo apt install -y terraform
    ```

##

### 6️⃣ Verificar a instalação do Terraform

- Confirme a instalação verificando a versão:
    ```bash
    terraform --version
    ```

