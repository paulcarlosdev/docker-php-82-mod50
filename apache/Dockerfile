FROM ubuntu:24.04

# Desactivar la interacción durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Actualizar los repositorios e instalar paquetes básicos
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y \
    ca-certificates \
    apt-transport-https \
    software-properties-common \
    lsb-release \
    curl \
    gnupg2 \
    && add-apt-repository ppa:ondrej/php -y

# Instalar PHP 8.2 y las extensiones necesarias
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y \
    php8.2 \
    libapache2-mod-php8.2 \
    php8.2-cli \
    php8.2-common \
    php8.2-mysql \
    php8.2-intl \
    php8.2-zip \
    php8.2-gd \
    php8.2-soap \
    php8.2-mbstring \
    php8.2-curl \
    php8.2-xml \
    php8.2-bcmath \
    php8.2-pgsql \
    libpq-dev \
    gcc \
    make \
    autoconf

# Configure apache for TYPO3
RUN a2enmod rewrite expires
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/servername.conf
RUN a2enconf servername
# Configure vhost for TYPO3
#COPY typo3.conf /etc/apache2/sites-available/
#RUN a2dissite 000-default
#RUN a2ensite typo3.conf

# Instalar las dependencias necesarias para PostgreSQL

# Actualiza e instala PostgreSQL
RUN apt-get update
RUN apt-get upgrade -y
#RUN apt-get install -y postgresql postgresql-contrib

COPY custom-php.ini /etc/php/8.2/apache2/conf.d/99-custom.ini
COPY custom-php.ini /etc/php/8.2/cli/conf.d/99-custom.ini

# Configurar PHP para mejor uso de RAM y CPU
#RUN sed -i 's/memory_limit = .*/memory_limit = 1024M/' /etc/php/8.2/apache2/php.ini && \
#    sed -i 's/max_execution_time = .*/max_execution_time = 3000/' /etc/php/8.2/apache2/php.ini && \
#    sed -i 's/upload_max_filesize = .*/upload_max_filesize = 1024M/' /etc/php/8.2/apache2/php.ini && \
#    sed -i 's/max_input_vars = .*/max_input_vars = 5000/' /etc/php/8.2/apache2/php.ini && \
#    sed -i 's/post_max_size = .*/post_max_size = 512M/' /etc/php/8.2/apache2/php.ini

#RUN sed -i 's/max_input_vars = .*/max_input_vars = 5000/' /etc/php/8.2/cli/php.ini
    

EXPOSE 80 443 
#5432

WORKDIR /var/www/html

#RUN rm index.html

HEALTHCHECK --interval=5s --timeout=3s --retries=3 CMD curl -f http://localhost || exit 1

CMD apachectl -D FOREGROUND 