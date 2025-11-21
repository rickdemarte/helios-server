# INSTALL

Este documento registra as mudanças feitas para compatibilizar esta cópia do Helios a partir da versão atual do repositório `benadida/helios-server` e descreve como reproduzir o ambiente de desenvolvimento
## Dependências de sistema e clonagem

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-dev \
  build-essential gcc libpq-dev libldap2-dev libsasl2-dev \
  gettext git
git clone https://github.com/rickdemarte/helios-server
cd helios-server
```

*** É muito importante rodar os comandos de dentro da pasta do helios-server ***

`libpq-dev` expõe `pg_config` usado pelo `psycopg2` e os pacotes `libldap2-dev` `libsasl2-dev` são necessários para compilar `python-ldap`. O `gettext` fornece `msgfmt`, exigido para `compilemessages`.

## Ambiente Python - Antes de dar qualquer comando python

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

## Banco de dados

*** IMPORTANTE: Considero aqui que você já tem o Docker instalado e seu usuário consegue "rodar" os comandos ***

Para testes basta um container Postgres:

```bash
mkdir -p pgdata # para que os dados do banco fiquem dentro da pasta do helios-server
# Usuário e senha de teste/dev
docker run -d \
  --name helios-postgres \
  -e POSTGRES_USER=helios_user \
  -e POSTGRES_PASSWORD=helios_pwd \
  -e POSTGRES_DB=helios \
  -p 5432:5432 \
  -v $(pwd)/pgdata:/var/lib/postgresql/data \
  postgres:16
```

Variáveis úteis para o Django:

*** O Django vai rodar atrás de um proxy, então precisa desativar o SSL (local) ***

```bash
export DATABASE_URL=postgres://helios_user:helios_pwd@localhost:5432/helios?sslmode=disable
export DATABASE_SSL_REQUIRE=0
```

## Configuração do projeto

```bash
# Exporta com localhost pra evitar acesso "externo" na porta sem SSL
export SECRET_KEY=dev-secret \
       DEBUG=1 \
       ALLOWED_HOSTS=localhost,127.0.0.1 \
       URL_HOST=http://localhost:8000 \
       AUTH_ENABLED_SYSTEMS=password \
       AUTH_DEFAULT_SYSTEM=password

python manage.py migrate
python manage.py collectstatic --noinput
python manage.py compilemessages -l pt_BR
# Criação do superuser é opcional
#python3 manage.py createsuperuser
```

Para habilitar o admin foi adicionado `path('admin/', admin.site.urls)` em
`urls.py`. Garanta que `python manage.py createsuperuser` seja executado.

## Usuários do Helios

O login por senha usa a tabela `helios_auth_user`. Crie usuários com:

```bash
docker exec -i helios-postgres psql -U helios_user -d helios <<'SQL'
INSERT INTO helios_auth_user
    (user_type, user_id, name, info, token, admin_p)
VALUES
    ('password', 'criador', 'Usuário Criador',
     '{"password":"senhacriador","name":"Usuário Criador"}',
     NULL, TRUE)
ON CONFLICT (user_type, user_id) DO UPDATE
    SET name = EXCLUDED.name,
        info = EXCLUDED.info,
        admin_p = EXCLUDED.admin_p;
SQL

```

## Execução (se for rodar depois de reiniciar)

```bash
source .venv/bin/activate #se ainda não estiver ativo
# Exporta com localhost pra evitar acesso "externo" na porta sem SSL
python manage.py runserver 127.0.0.1:8000
```

O painel do Django fica em `http://localhost:8000/admin/` e o Helios em `http://localhost:8000/auth/`.