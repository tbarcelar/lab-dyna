<h1>
Configuração do WSL e Instalação do Ubuntu
</h1>

## 🛠️ Passo a passo

### 1️⃣ Ativar o WSL

- Abra o **PowerShell** ou o **Prompt de Comando** como administrador.
- Execute o comando para habilitar o WSL:
    ```bash
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

##

### 2️⃣ Configurar o WSL para usar a Versão 2 (WSL 2)

- Habilite o suporte à plataforma de máquina virtual:
    ```bash
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
- Reinicie o computador.

- Após reiniciar, defina o WSL 2 como versão padrão abrindo o **PowerShell** novamente e executando:
    ```bash
    wsl --set-default-version 2
    ```

##

### 3️⃣ Instalar o Ubuntu

- Abra a **Microsoft Store** no Windows.
- Procure por "Ubuntu" e clique em **Instalar**.
- Após a instalação, abra o aplicativo **Ubuntu**.
- Configure o nome de usuário e a senha ao iniciar pela primeira vez.


