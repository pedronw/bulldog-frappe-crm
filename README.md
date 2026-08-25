# Bulldog Frappe CRM no EasyPanel

Configuração de produção do Frappe CRM para `bulldog.kairosdemand.com`.

## 1. Subir este conteúdo ao GitHub

Crie um repositório privado chamado `bulldog-frappe-crm` e envie os quatro
arquivos desta pasta. Não crie nem envie um arquivo `.env` real ao GitHub.

## 2. Configurar a origem no EasyPanel

No serviço Compose, selecione `Git` e configure:

- Repository: URL do repositório criado
- Branch: `main`
- Build Path: `/`
- Docker Compose File: `docker-compose.yml`
- Create .env file: ativado

Se o repositório for privado, copie a chave SSH mostrada pelo EasyPanel e
cadastre-a no GitHub em **Settings > Deploy keys > Add deploy key**, apenas com
permissão de leitura.

## 3. Configurar o Environment no EasyPanel

Use o modelo abaixo, substituindo as duas senhas:

```env
SITE_NAME=bulldog.kairosdemand.com
DB_PASSWORD=UMA_SENHA_FORTE_E_EXCLUSIVA
ADMIN_PASSWORD=OUTRA_SENHA_FORTE_E_EXCLUSIVA
```

Não use espaços, aspas, crase ou o caractere `$` nas senhas. Não coloque essas
senhas no GitHub.

As senhas podem aparecer nos logs caso um comando de instalação falhe. Se isso
acontecer durante uma primeira instalação ainda vazia, troque as duas senhas e
recrie somente os volumes dessa nova stack antes de tentar novamente.

## 4. Fazer o primeiro deploy

Clique em **Deploy** e aguarde. Na primeira execução, as imagens serão baixadas,
o banco será preparado e o site será criado. Isso pode levar alguns minutos.

É normal que `configurator` e `create-site` terminem com `Exited (0)` ou
`Completed`. Os serviços `frontend`, `backend`, `websocket`, `queue-short`,
`queue-long`, `scheduler`, `db`, `redis-cache` e `redis-queue` devem ficar ativos.

Se o deploy falhar, não apague o serviço nem os volumes. Abra os logs do serviço
que falhou e corrija a causa antes de redeployar.

## 5. Adicionar o domínio

Depois que o site for criado, abra **Domains** e adicione:

- Domain: `bulldog.kairosdemand.com`
- Internal service: `frontend`
- Target port: `8080`
- Internal protocol: `HTTP`
- HTTPS/Let's Encrypt: ativado

Abra `https://bulldog.kairosdemand.com/crm` e entre com:

- Usuário: `Administrator`
- Senha: o valor definido em `ADMIN_PASSWORD`

## Atualizações

Faça um backup antes de atualizar. Em seguida, clique em **Deploy** novamente.
A configuração usa a imagem oficial `ghcr.io/frappe/crm:stable` e preserva os
dados nos volumes nomeados. Não renomeie os serviços ou volumes após começar a
usar o CRM.
