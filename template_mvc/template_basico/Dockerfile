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
