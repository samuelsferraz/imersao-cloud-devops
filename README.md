# imersao-cloud-devops

## Introdução
### Aula 1: Crie ambiente reais com Docker e dê fim ao "na minha máquina funciona"

#### Contexto
Nesta primeira aula, você vai aprender a trabalhar com contêineres utilizando Docker, preparando o ambiente ideal para aplicar conceitos de CI/CD. Também vamos explorar como a inteligência artificial pode ajudar na documentação e otimização de processos com o Gemini Code Assist.

#### Conteúdos
- Criar uma imagem Docker de uma aplicação simples (como uma API em Python).
- Entender e resolver o clássico problema do “na minha máquina funciona”.
- Aprender o que são contêineres e como funcionam as imagens Docker.
- Executar aplicações localmente via Docker Run e Docker-Compose.
- Construir suas próprias imagens personalizadas usando um Dockerfile.
- Utilizar o Docker Compose para uma orquestração básica de múltiplos serviços.
---
#### Minhas anotações sobre a aula:
##### O que é Docker e para que serve?

   Docker é uma plataforma de código aberto para desenvolvimento, envio e execução de aplicações em contêineres. Fornecendo  portabilidade, flexibilidade e escabilidade em qualquer ambiente, seja local ou na nuvem. 

##### Instalação do docker
1. Acessar https://www.docker.com/.
2. Clicar em Download Docker Desktop
3. Selecionar o sistema operacional
4. Instalar.

##### O que eu preciso fazer para de fato melhorar a portabilidade para executar em outros sistemas? 
1. Criar arquivo Dockerfile (Arquivo de definição cpm instruções do que vai ser rodado)
2.  Definir a imagem base que será baixada do [Docker Hub](https://hub.docker.com/). PS: A imagem recomendada é sempre a alpine, visto que a imagem mais leve de programação e plataformas
```
#FROM [--platform=<platform>] <image> [AS <name>]
FROM python:3.12.11-alpine3.22
```
3. Definir o diretório de trabalho dentro do contêiner
```
WORKDIR /app
```
4. Copia o arquivo de requisitos e instala as dependências. Usamos --no-cache-dir para evitar o cache do pip, reduzindo o tamanho da imagem
```
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
```
5. Copia o restante do código da aplicação para o diretório de trabalho
```
COPY . .
```
6. Expôe a porta que a aplicação FastAPI irá rodar (padrão é 8000)

```
EXPOSE 8000
```
7. Comando para rodar a aplicação usando uvicorn
```
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

### Fazer Build da imagem

```
docker build -t api .
```
### Remover Imagem
```
docker rmi api:latest
```

### Rodar Imagem
```
docker run -p 8000:8000 api
```
### Aula 2: Aprenda CI/CD na prática: automatize do build ao deploy

#### Contexto
Nesta aula, vamos implementar uma pipeline CI/CD básica no GitLab, usando o projeto Docker criado anteriormente. Você também vai conhecer os principais componentes de uma pipeline e aprender como a IA pode apoiar na criação de testes automatizados.

#### Conteúdos
- Entender o que é CI/CD na prática.
- Explorar os principais componentes de uma pipeline no GitLab: estágios, jobs, runners e o arquivo .gitlab-ci.yml.
- Construir uma pipeline simples com as etapas de build, test e deploy.
- Utilizar variáveis de ambiente e integrar o pipeline com o Docker.
---
#### Minhas anotações sobre a aula:

```
docker compose up
```

### Aula 3: Fazendo deploy na Google Cloud Platform

1. Instalar gcloud CLI: https://cloud.google.com/sdk/docs/install

```
gcloud auth login
gcloud config set project PROJECT_ID
gcloud run deploy --port=8000
```

