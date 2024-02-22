# Curso PHP 2024

## Template básico


Para usar este template para aproveitar os middleware já definidos ou pode deixar eles como **guia de desenvolvimento**

+ ./app/Http/Controllers/Controller.php
+ ./app/Http/Controllers/HomeController.php
+ ./app/Http/Controllers/AuthController.php
+ ./app/Http/Controllers/ModeloController.php
+ ./app/Http/Controllers/MarcaController.php
+ ./app/Http/Controllers/CarroController.php
+ ./app/Http/Controllers/ClienteController.php
+ ./app/Http/Controllers/LocacaoController.php
+ ./app/Http/Controllers/Auth/ConfirmPasswordController.php
+ ./app/Http/Controllers/Auth/RegisterController.php
+ ./app/Http/Controllers/Auth/ResetPasswordController.php
+ ./app/Http/Controllers/Auth/ForgotPasswordController.php
+ ./app/Http/Controllers/Auth/VerificationController.php
+ ./app/Http/Controllers/Auth/LoginController.php

e com os Models:

+ ./app/Models/Modelo.php
+ ./app/Models/Locacao.php
+ ./app/Models/Marca.php
+ ./app/Models/User.php
+ ./app/Models/Cliente.php
+ ./app/Models/Carro.php

Antes de iniciar sua codificação se lembre de versionar (github) e de testar.

```
php artisan test

vendor/bin/phpunit
```

# Dicas para acelerar o desenvolvimento:

+ Se for preciso criar um PMV pode-se fazer o login (numa instancia EC2 da aws) usando o serviço **AWS Cognito** (https://aws.amazon.com/pt/cognito/) 




























