# Manual do Desenvolvedor

## Script 1

O Script 1 tem como objetivo iniciar o comando que você deseja em todos os serviços que estão descritos na Lista de Serviços do script. 

### Passos:

- Realizar o comando "git clone" dos repositórios que estão descritos em "Lista de Serviços" no script.
- Crie um arquivo chamado "start_containers.sh" dentro do repositório "2023-2-CAPJu-Doc" e cole o script.
- Escreva o comando que você deseja no tópico "Inicia o serviço de configuração" (O comando "docker-compose up -d" é apenas um exemplo de comando, você escolhe qual comando você deseja).

# !/bin/bash

# Lista de serviços.
services=(
  "2023-2-CAPJu-Config"
  "2023-2-CAPJu-Note-Service"
  "2023-2-CAPJu-ProcessManagement-Service"
  "2023-2-CAPJu-Role-Service"
  "2023-2-CAPJu-Unit-Service"
  "2023-2-CAPJu-User-Service"
)

# Inicia o serviço de configuração.
cd "2023-2-CAPJu-Config"  # Entrar na pasta
  docker-compose up -d
cd ..  # Voltar à pasta anterior

# Espera até que o serviço de configuração esteja online.
while ! docker ps | grep -q db; do
  sleep 1
done

# Inicia os demais serviços.
for service in "${services[@]}"; do
  if [ "${service}" != "2023-2-CAPJu-Config" ]; then
    cd "${service}"
    docker-compose up -d
    cd ..
  fi
done