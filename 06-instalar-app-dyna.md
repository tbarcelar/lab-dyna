<h1>
    <a href="https://developer.dynatrace.com/">
     <img align="center" width="40px" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Dynatrace_Logo.svg/512px-Dynatrace_Logo.svg.png">
    </a>
    <span> Dynatrace App Configuration Guide</span>
</h1>

## Preparando o Ambiente Dynatrace no WSL

### Instalar e Configurar Node.js

- Instale a versão 20 do Node.js usando o NVM:
    ```bash
    nvm install 20
    ```
- Use a versão instalada do Node.js:
    ```bash
    nvm use 20
    ```
- Defina a versão instalada como padrão:
    ```bash
    nvm alias default 20
    ```

##

### Verificar a Instalação do Node.js no WSL

- Verifique se o Node.js está instalado corretamente:
    ```bash
    node -v
    ```

##

### Garantir que o Caminho do Node.js Está no PATH

- Abra o arquivo `~/.bashrc` para edição:
    ```bash
    nano ~/.bashrc
    ```
- Adicione a linha abaixo ao final do arquivo:
    ```bash
    export PATH=$PATH:/usr/local/bin
    ```
- Salve e feche o arquivo:
  - Pressione `Ctrl + O` para salvar.
  - Pressione `Ctrl + X` para sair.

##

### Recarregar o bashrc

- Recarregue o arquivo `~/.bashrc` para aplicar as mudanças:
    ```bash
    source ~/.bashrc
    ```

##

### Instalar o Dynatrace App Toolkit

- Instale o Dynatrace App Toolkit globalmente:
    ```bash
    npm install -g dt-app
    ```
- Crie um novo app Dynatrace no terminal:
    ```bash
    npx dt-app@latest create --environment-url https://{id-tenant}.apps.dynatrace.com my-app-dynatrace
    ```

##

### Iniciar o Servidor de Desenvolvimento

- Navegue até o diretório do seu projeto Dynatrace:
    ```bash
    cd ~/projeto-dyna/dashboard-app
    ```
- Inicie o servidor de desenvolvimento:
    ```bash
    dt-app dev
    ```

Após seguir esses passos, o servidor de desenvolvimento abrirá o navegador para o ambiente de desenvolvimento do Dynatrace.


