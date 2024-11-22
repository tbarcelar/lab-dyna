<h1>
Instalação do Dynatrace OneAgent com Terraform
</h1>

## 🛠️ Passo a passo

### 1️⃣ Configuração do arquivo Terraform

- Crie um arquivo de configuração Terraform chamado `dynatrace_oneagent.tf` com o seguinte conteúdo:
    ```hcl
    provider "local" {}

    resource "local_file" "dynatrace_install_script" {
      content = <<EOF
      #!/bin/bash
      wget -O Dynatrace-OneAgent.sh "https://{id}.live.dynatrace.com/api/v1/deployment/installer/agent/unix/default/latest?arch=x86" --header="Authorization: Api-Token {token}}"
      sudo /bin/bash Dynatrace-OneAgent.sh --set-app-log-content-access=true --set-host-name=wsl
      EOF

      filename = "${path.module}/dynatrace_install.sh"
      file_permission = "0777"  # Permissão para execução
    }

    resource "null_resource" "install_oneagent" {
      depends_on = [local_file.dynatrace_install_script]

      provisioner "local-exec" {
        command = "bash ${path.module}/dynatrace_install.sh"
      }
    }
    ```

- Substitua os valores `{id}`, `{token}`, `<your-dynatrace-environment>`, `<your-api-token>` e `<your-environment-id>` pelos dados específicos do seu ambiente Dynatrace.

##

### 2️⃣ Inicializar o Terraform

- Execute o comando abaixo para inicializar o Terraform:
    ```bash
    terraform init
    ```

##

### 3️⃣ Aplicar a configuração do Terraform

- Aplique a configuração com o comando:
    ```bash
    terraform apply
    ```

- Revise as mudanças que o Terraform fará e confirme digitando `yes` quando solicitado.

##

### 4️⃣ Verificar a instalação

- Verifique se o Dynatrace OneAgent foi instalado corretamente:
    ```bash
    sudo systemctl status dynatrace-agent
    ```

