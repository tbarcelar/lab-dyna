<h1>
Instalação do Docker no Ubuntu
</h1>

## 🛠️ Passo a passo

### 1️⃣ Atualizar pacotes do sistema

- Atualize os pacotes do sistema com o comando:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

##

### 2️⃣ Instalar dependências necessárias

- Instale os pacotes necessários:
    ```bash
    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
    ```

##

### 3️⃣ Adicionar chave GPG do repositório oficial do Docker

- Adicione a chave GPG do Docker:
    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

##

### 4️⃣ Adicionar o repositório Docker à lista de fontes

- Configure o repositório oficial do Docker:
    ```bash
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

##

### 5️⃣ Instalar o Docker

- Atualize a lista de pacotes e instale o Docker:
    ```bash
    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io -y
    ```

##

### 6️⃣ Verificar a versão do Docker instalada

- Confirme a instalação verificando a versão:
    ```bash
    docker --version
    ```

##

### 7️⃣ (Opcional) Usar o Docker sem sudo

- Adicione o usuário atual ao grupo `docker`:
    ```bash
    sudo usermod -aG docker $USER
    ```

Após executar este passo, saia e entre novamente no terminal para aplicar as mudanças.

