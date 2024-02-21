# Curso PHP 2024

# Atendimento de uma falha de segurança contra uma EC2 aws rodando um App Laravel

As instâncias EC2 da AWS poucas vezes são atacadas por "piratadas informáticos", mas no caso de um ataque geralmente o App fica severamente comprometido.

No caso de um APP do tipo ERP, ou sejaum aplicativo com bastante código, recomendo o uso de containers (https://github.com/leoqbc/php8-aws-lambda-runtime/blob/main/README.md) para poder
construir um ambiente local que pode ser colocado na cloud com ajuda do serviço ECR (https://www.youtube.com/watch?v=QsgUaMlsnEc)

```bash

laravel new api-julian
cd api-julian
php artisan serve &
curl http://localhost:8000/
```

