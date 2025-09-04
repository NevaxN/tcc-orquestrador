# TCC - Ambiente de Desenvolvimento Orquestrado
Bem-vindo ao repositório orquestrador do nosso TCC. Este projeto centraliza o ambiente de desenvolvimento, permitindo que qualquer integrante da equipe suba a aplicação completa — frontend, backend, serviço de IA e banco de dados — com um único comando.

O objetivo é eliminar problemas de configuração local e garantir que todos trabalhem em um ambiente padronizado e consistente, utilizando Docker.

## ⚙️ Arquitetura
Este repositório funciona como um orquestrador. Ele não contém o código-fonte das aplicações, mas sim:

1. O arquivo ```docker-compose.yml``` que define e conecta todos os serviços.

2. Links (via Git Submodules) para os repositórios de cada serviço.

Os serviços são:

```app-frontend-TCC```: A interface de usuário em Next.js.

```app-backend-TCC```: A API principal em Java/Spring Boot.

```keyword-extractor-TCC```: O microsserviço em Python para extração de palavras-chave.

## 📚 Como Começar (Setup Inicial)
Para configurar e rodar o ambiente pela primeira vez, siga estes passos:

**1. Clonar o Repositório Orquestrador**

Abra seu terminal e clone este repositório usando o comando ```--recurse-submodules```. Isso garante que o repositório principal e todos os submódulos sejam baixados de uma vez.

```bash
git clone --recurse-submodules <URL_DO_REPOSITORIO_ORQUESTRADOR>
```

**2. Navegar para a Pasta do Projeto**

```bash
cd tcc-orquestrador
```

**3. Subir o Ambiente com Docker Compose**

Este comando irá construir as imagens Docker de cada serviço (se ainda não existirem) e iniciar todos os contêineres em modo de desenvolvimento.

```bash
docker compose up --build
```

**4. Verificar se Tudo está Funcionando**

Aguarde até que os logs no terminal se estabilizem. Você verá as saídas de todos os serviços. Para acessar a aplicação, abra seu navegador em:

- Frontend: ```http://localhost:3000```

As outras portas expostas (para testes diretos de API, se necessário) são:

- Backend (API Java): ```http://localhost:8080```

- Serviço Python: ```http://localhost:5000```

- PostgreSQL: ```localhost:5432```

## ☀️ Fluxo de Trabalho Diário
Depois do setup inicial, seu dia a dia será muito mais simples.

**Para iniciar o ambiente:**
Navegue até a pasta tcc-orquestrador e rode:

```bash 
docker compose up
```

(O ```--build``` só é necessário se você alterar um ```Dockerfile``` ou alguma dependência de build, como o ```build.gradle``` ou ```package.json```).

Para parar o ambiente:
No terminal onde o Docker Compose está rodando, pressione ```Ctrl + C```. Para garantir que os contêineres e a rede sejam removidos, rode:

```bash
docker compose down
```

**Editando o Código (Hot-Reload):**
Graças aos volumes configurados no ```docker-compose.yml```, você pode simplesmente abrir as pastas dos submódulos (```app-frontend-TCC```, ```app-backend-TCC```, etc.) no seu editor de código preferido (VS Code, IntelliJ). Qualquer alteração que você salvar no código-fonte será refletida automaticamente dentro do contêiner, reiniciando o serviço correspondente.

## 🌱 Gerenciando os Submódulos
**Para atualizar seu ambiente com as últimas alterações de todos os projetos:**

1. Primeiro, puxe as atualizações do orquestrador:

```bash
git pull
```
2. Em seguida, execute o comando para que os submódulos sincronizem com as versões mais recentes registradas no orquestrador:

```bash
git submodule update --init --remote --merge
```
**Para enviar suas próprias alterações:**
Lembre-se que o fluxo de trabalho com submódulos exige dois commits:

1. **Commit no submódulo:** Entre na pasta do serviço que você modificou (ex: ```cd app-backend-TCC```), faça seu ```git add```, ```git commit``` e ```git push``` normalmente.

2. **Commit no orquestrador:** Volte para a pasta raiz (cd ..), e você verá que o submódulo modificado aparece como uma alteração. Faça ```git add``` e ```git commit``` para "salvar" a nova referência ao commit que você acabou de criar.

## ⚠️ Troubleshooting (Solução de Problemas)
- **Erro** ./gradlew: not found n**o Windows**:

  - **Causa**: Finais de linha (CRLF vs LF).

  - **Solução**: Este problema já deve estar resolvido pelo arquivo ```.gitattributes``` no repositório do backend, que força o uso de finais de linha no padrão Linux (LF). Se o erro persistir, verifique se seu Git está configurado corretamente.

- **Portas em Conflito:**

  - **Causa**: Outro serviço na sua máquina já está usando uma das portas (```3000```, ```8080```, ```5432```, etc.).

  - **Solução**: Você pode alterar a porta do hospedeiro no ```docker-compose.yml```. Por exemplo, para ```8081:8080```, o contêiner continuará usando a porta 8080, mas você a acessará via ```localhost:8081```.
