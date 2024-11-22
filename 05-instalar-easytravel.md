<h1>
Instalação do Easytravel através do Docker
</h1>

## 🛠️ Passo a passo

### 1️⃣ Clonar o ambiente Easytravel

- Clone o repositório do Easytravel usando o comando:
    ```bash
    sudo git clone https://github.com/Dynatrace/easyTravel-Docker
    ```

##

### 2️⃣ Iniciar o ambiente Docker

- Acesse o diretório criado pelo clone e inicie o ambiente:
    ```bash
    cd easyTravel-Docker
    docker-compose up
    ```

##

### 3️⃣ Acessar os serviços do Easytravel

- Frontend:
    - [http://localhost:54428/](http://localhost:54428/)
- Frontend Angular:
    - [http://localhost:54429/easytravel/home](http://localhost:54429/easytravel/home)
- Backend:
    - [http://localhost:8091/](http://localhost:8091/)

##

### 4️⃣ Parar o ambiente Docker

- Para parar o ambiente, execute:
    ```bash
    docker-compose stop
    ```

