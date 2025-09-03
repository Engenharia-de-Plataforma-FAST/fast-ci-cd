# FAST CI/CD

Sejam bem-vindos!! Este é o repositório dos laboratórios de CI/CD do FAST!

## Pipelines

[![Actions status](https://github.com/HardSource/fast-ci/actions/workflows/ci.yml/badge.svg)](https://github.com/HardSource/fast-ci/actions/workflows/ci.yml)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/166f2da2726f417f96877ff592082837)](https://app.codacy.com/gh/HardSource/fast-ci/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)
[![Actions status](https://github.com/HardSource/fast-ci/actions/workflows/cd.yml/badge.svg)](https://github.com/HardSource/fast-ci/actions/workflows/cd.yml)

## Referências

Este repositório é um aplicativo de exemplo para usuários que seguem o guia de primeiros passos em https://docs.docker.com/get-started/.

O aplicativo é baseado no aplicativo do tutorial de introdução em https://github.com/docker/getting-started


## Requisitos para utilizar:
1. Ter o node + yarn instalado na máquina (https://nodejs.org/pt-br/download/)
2. (Opcional, mas bem útil) instalar o VSCode (https://code.visualstudio.com/download)

## Para rodar o exemplo localmente:
1. Dentro da pasta, abra o terminal e rode o seguinte comando para instalar as dependências:
```bash
$ yarn install
```

2. Para iniciar o servidor, basta rodar:

```bash
$ yarn start
```

3. Assim que iniciar, ele deve abrir uma aba do seu navegador favorito. Se não, acesse:
   * http://localhost:3000


# Configuração de Secrets

Para que as pipelines de CI/CD funcionem corretamente, é necessário configurar os seguintes secrets no seu repositório do GitHub. Acesse `Settings > Secrets and variables > Actions` e adicione os secrets abaixo.

### `TOKEN_GITHUB`

-   **Descrição**: Um *Personal Access Token (PAT)* do GitHub. É utilizado para permitir que a pipeline interaja com a API do GitHub, especificamente para:
    1.  Publicar um comentário no Pull Request com as tags da imagem Docker gerada.
    2.  Disparar o workflow de CD (`cd.yml`) a partir da pipeline de CI.
-   **Como gerar**:
    1.  Acesse seu perfil do GitHub > `Settings` > `Developer settings` > `Personal access tokens` > `Fine-grained tokens`.
    2.  Clique em `Generate new token`.
    3.  Dê um nome para o token (ex: `fast-ci-cd-actions`).
    4.  Selecione o repositório onde a Action será executada.
    5.  Em `Repository permissions`, conceda as seguintes permissões:
        -   `Actions`: `Read and write` (para disparar outros workflows).
        -   `Pull requests`: `Read and write` (para publicar comentários).
    6.  Clique em `Generate token`, copie o valor gerado e adicione ao repositório com o nome `TOKEN_GITHUB`.

### `DOCKERHUB_USERNAME` e `DOCKERHUB_TOKEN`

-   **Descrição**: Credenciais para autenticação no Docker Hub. São necessárias para fazer o login e publicar a imagem Docker construída.
-   **Como gerar**:
    1.  **`DOCKERHUB_USERNAME`**: Adicione seu nome de usuário do Docker Hub como um secret.
    2.  **`DOCKERHUB_TOKEN`**:
        -   Acesse sua conta no [Docker Hub](https://hub.docker.com).
        -   Clique no seu perfil > `Account Settings` > `Personal access tokens`.
        -   Clique em `Generate new token`.
        -   Dê uma descrição (ex: `GitHub Actions Token`) e defina as permissões `Read, Write, Delete`.
        -   Clique em `Generate`, copie o token e adicione ao repositório com o nome `DOCKERHUB_TOKEN`.

### `SONAR_TOKEN`

-   **Descrição**: Token de autenticação para o SonarCloud. É utilizado para enviar os resultados da análise de código estático para o seu projeto no SonarCloud.
-   **Como gerar**:
    1.  Faça login no [SonarCloud](https://sonarcloud.io/).
    2.  Acesse seu perfil > `My Account` > `Security`.
    3.  Em `Generate Tokens`, digite um nome para o token (ex: `github-fast-ci`) e clique em `Generate`.
    4.  Copie o token gerado e adicione-o ao repositório com o nome `SONAR_TOKEN`.

### `SSH_KEY`

-   **Descrição**: Chave SSH privada para acessar a máquina virtual (VM) de deployment. A pipeline de CD utiliza esta chave para copiar arquivos (`.env`) e executar comandos remotos (como gerenciar serviços Docker).
-   **Como gerar**:
    1.  Em sua máquina local, gere um novo par de chaves SSH (se ainda não tiver uma dedicada para isso):
        ```bash
        ssh-keygen -t ed25519 -C "fast-cicd"
        ```
        *Dica: Não adicione uma passphrase (senha) à chave para que a automação funcione sem interrupção.*
    2.  Isso criará dois arquivos: `id_ed25519` (chave privada) e `id_ed25519.pub` (chave pública).
    3.  Adicione a **chave pública** (`id_ed25519.pub`) ao arquivo `~/.ssh/authorized_keys` na sua VM de deployment.
    4.  Copie o conteúdo da **chave privada** (`id_ed25519`).
    5.  Adicione este conteúdo ao repositório com o nome `SSH_KEY`.

### `WEBHOOK_URL`

-   **Descrição**: URL de um Webhook do Discord. É utilizado na pipeline de CD para enviar notificações sobre o status do deployment para um canal específico.
-   **Como gerar**:
    1.  No Discord, vá para as configurações do servidor onde deseja receber as notificações.
    2.  Acesse `Integrações` > `Webhooks` > `Novo Webhook`.
    3.  Defina um nome para o webhook e escolha o canal.
    4.  Clique em `Copiar URL do webhook`.
    5.  Adicione a URL copiada ao repositório com o nome `WEBHOOK_URL`.

### `MY_SUPER_SECRET`

-   **Descrição**: Um secret de exemplo utilizado na pipeline de CD para demonstrar a criação de um arquivo `.env` com variáveis de ambiente.
-   **Como gerar**:
    -   Este é um valor de teste. Você pode adicionar qualquer string de sua preferência. Por exemplo, `meu-valor-secreto-123`.
