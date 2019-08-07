Este metodo es solo por la via de "make make install" no por paquetes 
ya que no hay paquete para alpine.  **El documento es un WIP**

## Instalacion Redmine

#### paquetes necesarios

Para ejecutar despues de compilar:

```
apk upgrade --no-cache && \
    apk add --no-cache \
      sudo \
      ruby ruby-bundler ruby-bigdecimal \
      ruby-rmagick \
      ruby-json \
      libc6-compat \
      imagemagick \
      tzdata
```

Para compilar y tener el redmine:

```
apk add --no-cache \
  build-base \
  ruby-dev \
  zlib-dev \
  cmake \
  mariadb-dev \
  coreutils \
  git
```

#### Descargar redmine y compilar

```
cd /home/

TAG=`git ls-remote -t https://github.com/redmine/redmine.git | grep -v -e '\^{}' -e 'rc[0-9]*' -e 'pre' | grep -o '[0-9]\..*$' | tail -1`

sudo -u redmine -H git clone --depth 1 -b ${TAG} https://github.com/redmine/redmine.git redmine -v

cd /home/redmine/

sudo -u redmine -H echo "install: --no-document" > ~/.gemrc

sudo -u redmine -H cp config/database.yml.example config/database.yml

bundle install --without development test -j$(nproc)

chown -R redmine:redmine files log tmp public/plugin_assets
chmod -R 755 files log tmp public/plugin_assets
```

#### Remover esas toolchains

```
apk del   build-base \
  ruby-dev \
  zlib-dev \
  cmake \
  mariadb-dev \
```

#### Instalar el runtime

TODO: mejor manera de hacer esto en servidor


```
RUNDEP=`scanelf --needed --nobanner --format '%n#p' --recursive /usr/lib/ruby | tr ',' '\n' | sort -u | awk 'system("[ -e /lib/" $1 " -o -e /usr/lib/" $1 " -o -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }'`

apk add --no-cache $RUNDEP
```

## Configurar el Redmine

Se asume mysql configurado y ajustado listo para usar:

#### configuracion db y ejecucion en puerto

```
# set default
MYSQL_HOST=${DB_HOST:-redmine-mariadb}
MYSQL_DATABASE=${DB_NAME:-redmine}
MYSQL_USER=${DB_USER:-redmine}
MYSQL_PASSWORD=${DB_PASS:-redminepass}

cd /home/redmine

cat <<EOF > config/database.yml
production:
  adapter: mysql2
  database: $MYSQL_DATABASE
  host: $MYSQL_HOST
  username: $MYSQL_USER
  password: $MYSQL_PASSWORD
  encoding: utf8
EOF

chown -R redmine:redmine .

bundle exec rake db:migrate RAILS_ENV="production" --trace

rake generate_secret_token --trace

exec sudo -u redmine -H ruby bin/rails server -e production -b 0.0.0.0

```
