# Configuração do WSL e Instalação do Ubuntu

Este guia descreve como configurar o **Windows Subsystem for Linux (WSL)**, definir a versão 2 como padrão e instalar o **Ubuntu**.

---

## 🛠️ Passo a passo

### 1️⃣ Ativar o WSL
1. Abra o **PowerShell** ou o **Prompt de Comando** como administrador.
2. Execute o comando para habilitar o WSL:
   ```bash
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

2️⃣ Configurar o WSL para usar a Versão 2 (WSL 2)

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

Reinicie o computador.

Após reiniciar, abra o PowerShell novamente. Defina o WSL 2 como versão padrão
wsl --set-default-version 2


3️⃣ Instalar o Ubuntu

Abra a Microsoft Store no Windows.
Procure por "Ubuntu" e clique em Instalar.
Após a instalação, abra o aplicativo Ubuntu.
Configure o nome de usuário e a senha ao iniciar pela primeira vez.

