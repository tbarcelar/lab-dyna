# Instalação do Easytravel através do docker
Este guia descreve como ter um ambiente completo com banco, applicação etc, para se concentrar somente no dynatrace. 


🛠️ Passo a passo

$ Clonar o ambiente
sudo git clone https://github.com/Dynatrace/easyTravel-Docker


$ Entra no diretorio criado do clone e inicia o ambiente
docker-compose up


$ Frontend
http://localhost:54428/


$ Frontend angular
http://localhost:54429/easytravel/home


$ backend
http://localhost:8091/



$ Parar o compose
docker-compose stop

