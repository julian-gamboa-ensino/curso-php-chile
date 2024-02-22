# Curso PHP 2024

# Atendimento de uma falha de segurança contra uma EC2 aws rodando um App Laravel

As instâncias EC2 da AWS poucas vezes são atacadas por "piratadas informáticos", mas no caso de um ataque geralmente o App fica severamente comprometido.

No caso de um APP do tipo **ERP**, ou sejau m aplicativo com bastante código, recomendo o uso de containers (https://github.com/leoqbc/php8-aws-lambda-runtime/blob/main/README.md) para poder
construir um ambiente local que pode ser colocado na cloud com ajuda do serviço ECR (https://www.youtube.com/watch?v=QsgUaMlsnEc)


# Preparação de um ambiente básico


Para preparar um ambiente local de uma API laravel pode-se usar a sequencia de comandos (bash shell): 

```bash

laravel new api-julian
cd api-julian
php artisan serve &
curl http://localhost:8000/
```

Ou no caso de Docker recomendo usar o **sail** https://laravel.com/docs/master/sail (Uma maneira fácil de iniciar e parar o ambiente de desenvolvimento Docker), e no caso de querer aqprender mais recomendo o tutorial: https://martinjoo.dev/devops-with-laravel-dockerizing-a-laravel-application no qual se explica tanto a Dockerização de uma API como de uma aplicação WEB com VUE.


Neste ambiente básico podem se observar os log 

```bash
tail s storage/logs/laravel.log 
```
O que pode ser feito de forma microscopica (https://laravel.com/docs/10.x/logging). Observar os logs seja na sua forma leve (log configuraddo como "debug"), ou nas suas formas mais avançadas: info, warning, error e critical.

## Exemplo interessante: Observar o log das migrações

Usando a receita: 

```bash


composer create-project --prefer-dist laravel/laravel blog

cd blog

php artisan make:model Usuario -m
php artisan make:model Postagem -m
php artisan make:model Comentario -m



# No arquivo .end deve colocar: "DB_CONNECTION=sqlite"

php artisan migrate


php artisan make:controller UsuarioController --resource
php artisan make:controller PostagemController --resource
php artisan make:controller ComentarioController --resource

```

E no projeto laravel coloque as informaçoes:

```php

use App\Http\Controllers\PostagemController;
use App\Http\Controllers\ComentarioController;
use App\Http\Controllers\UsuarioController;

//......................

Route::get('/', function () {
    return view('welcome');
});

Route::get('/usuarios', [UsuarioController::class, 'index']);
Route::get('/postagens', ['PostagemController::class', 'index']);
Route::get('/comentarios', ['ComentarioController::class', 'index']);

```
No qual pode-se ver como saída do log:

```php
(Illuminate\\Database\\QueryException(code: 0): could not find driver (Connection: mysql, SQL: select table_name as `name`, (data_length + index_length) as `size`, table_comment as `comment`, engine as `engine`, table_collation as `collation` from information_schema.tables where table_schema = 'laravel' and table_type in ('BASE TABLE', 'SYSTEM VERSIONED') order by table_name) 

```

## Docker : colocar os logs num volume local

Usando o arquivo Dockerfile:

```
# Usa a imagem base php:8.2-fpm do Docker Hub
FROM php:8.2-fpm

# Instala as dependências necessárias para o Composer
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip

# Instala o Composer
RUN curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Define o diretório de trabalho dentro do contêiner como /app
WORKDIR /app

# Copia todos os arquivos do diretório local para o diretório de trabalho do contêiner (/app)
COPY . .

# Executa o comando 'composer install' para instalar as dependências do Composer
RUN composer install

# Executa o comando 'php artisan key:generate' para gerar a chave de criptografia do Laravel
RUN php artisan key:generate

# Define as permissões de leitura, escrita e execução recursivas (775) para o diretório de armazenamento (storage) do Laravel
RUN chmod -R 775 storage

# Expõe a porta 8000 do contêiner para acessar a aplicação Laravel
EXPOSE 8000

# Define o comando padrão a ser executado quando o contêiner for iniciado, que é 'php artisan serve' com host '0.0.0.0' e porta '8000'
CMD ["php", "artisan", "serve", "--host", "0.0.0.0", "--port", "8000"]


```


No qual pode-se defnir um volume local para guardar os logs na forma: 

```
VOLUME /var/log/laravel
```

# Template simples:

Este template pode te ajudar para desenvolver uma aplicação tradicional (blog,comercio eletrônico, loja, etc.).  Para este curso recomendo usar o método TDD:

```bash
vendor/bin/phpunit --whitelist src/ --coverage-html cover/
```

Assim como o uso de um Jenkis para automação da esteira CI/CD

```bash
pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'composer install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'vendor/bin/phpunit'
            }
        }
        stage('Build and Deploy') {
            steps {
                sh 'php artisan migrate --force'
                sh 'php artisan cache:clear'
            }
        }
    }
}
```

    























