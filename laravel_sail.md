# Laravel sail

## What's that?

Default Docker development environment in Laravel. You do not need to install the softwares and libraries on your local computer, and do not need to setup the virtual machine such as Homestead to have a convenient for development.

## Prerequisites

1. Docker daemon on your OS.

2. For Windows, WSL is required.

## Create a new Laravel project with sail

1. Run the following commands to create a new Laravel project with sail:

```bash
# development environment
curl -s "http://laravel.test/<PROJECT_NAME>" | bash
```
or
```bash
# production environment
curl -s "http://laravel.build/<PROJECT_NAME>" | bash
```

2. Build the docker image by following command:

```bash
./vendor/bin/sail up -d
```

3. Access ```http://locahost``` on browser, you will see the application is running.


## Install Laravel sail to the existing project

1. Clone your repository from Git if you have. 

2. Run the following commands to install the package of Laravel sail:

```bash
# install the package
composer require laravel/sail --dev
```
**Remark: ```--dev``` for service name ```laravel.test``` and ```--build``` for ```laravel.build```**

and install sail to your laravel project

```bash
# install sail to laravel project
php artisan sail:install
```

After that, ```docker-compose.yml``` was created to the root directory of the project.

3. Modify your ```.env``` file with environment variables as the application needs.

4. Build the docker image by following command:

```bash
./vendor/bin/sail up -d
```

5. Access ```http://locahost``` on browser, you will see the application is running.

## (Optional) Add alias for Laravel sail via Bash profile or Z shell

### 1. For bash user

1. Run ```nano ~/.bash_profile``` and add the following command to this file.

Add the following command on 

```bash
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```
then save and quit.

2. Run ```source ~/.bash_profile```

### For Z shell user

1. Run ```vim ~/.zshrc``` and add the following command to this file,

```zsh
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```
then save and quit.

2. Run ```source ~/.zshrc```

### Start up the project

1. Run ```sail up``` to start up the project

**You can type `sail <COMMAND>` access the laravel sail everywhere!**

### (Optional) If your project using vite as asset bunbling tool, run the following command at new terminal:

```bash
# For development and enable HMR
sail yarn run dev
```
or
```bash
# For production
sail yarn run build
```

## (Optional) Add MongoDB service to Laravel sail

1. Install package
```bash
sail composer require mongodb/laravel-mongodb
```

2. Add dependence for MongoDB service on ```docker-compose.yml```

```yml
laravel.test:
    # ...
    depends_on:
        - mongo
    #...
```

3. Add MongoDB service on Docker container by adding following content on ```docker-compose.yml```

```yml
mongo:
    image: 'mongo:4.4'
    restart: always
    environment:
        MONGO_INITDB_ROOT_USERNAME: '${MONGO_DB_USERNAME}'
        MONGO_INITDB_ROOT_PASSWORD: '${MONGO_DB_PASSWORD}'
        MONGO_INITDB_DATABASE: '${MONGO_DB_DATABASE}'
    volumes:
        - 'sailmongo:/data/db'
    networks:
        - sail
```

4. Declare ```volumn``` on the same file

```yml
volumes:
    # ...
    sailmongo:
        driver: local
    # ...

```

5. Add configuration for mongodb on ```config/database.php```

```php
'mongodb' => [ 
    'driver' => 'mongodb', 
    'host' => env('MONGO_DB_HOST', 'mongo'), 
    'port' => env('MONGO_DB_PORT', '27017'), 
    'database' => env('MONGO_DB_DATABASE') , 
    'username' => env('MONGO_DB_USERNAME'), 
    'password' => env('MONGO_DB_PASSWORD'), 
    'options' => [ 
        'database' => env('DB_AUTHENTICATION_DATABASE', 'admin'), 
    ], 
]
```

6. Add the following configurations on ```.env``` files. 

For example,

```
MONGO_DB_HOST = <YOUR_DATABASE_HOST>
MONGO_DB_PORT = <YOUR_DATABASE_PORT>
MONGO_DB_DATABASE = <YOUR_DATABASE_NAME>
MONGO_DB_USERNAME = <YOUR_DATABASE_USERNAME>
MONGO_DB_PASSWORD = <YOUR_DATABASE_PASSWORD>
```

7. Run ```sail root-shell``` to arbitrary shell commands within the container. Then, run ```pecl install mongodb``` to install mongodb binary in the container.

8. Run ```echo "extension=mongodb.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"` ``` to Add extension to ```php.ini```, then ```exit``` to exit the shell.

9. Run ```sail artisan sail:publish``` to generate a new folder ```docker``` will be created named docker. 

Inside the folder ```docker```, the folders named with version numbers that corresponds to your PHP version that you are using in your container.

10.  If you are using PHP 8.1, choose the folder ```8.1``` and open the ```Dockerfile``` add the mongodb php extension inside as follow:

```
# ...
&& apt-get install -y php8.1-cli php8.1-dev \
    php8.1-pgsql php8.1-sqlite3 php8.1-gd php8.1-imagick \
    php8.1-curl \
    php8.1-imap php8.1-mysql php8.1-mbstring \
    php8.1-xml php8.1-zip php8.1-bcmath php8.1-soap \
    php8.1-intl php8.1-readline \
    php8.1-ldap \
    php8.1-msgpack php8.1-igbinary php8.1-redis php8.1-swoole \
    php8.1-memcached php8.1-pcov php8.1-xdebug \
    php8.1-mongodb \ # <- Add this
# ...
```

11. Run the following commands build our container again

```bash
#Stop sail
sail stop

#Rebuild container
sail build

#Start sail
sail sail up -d

#Install the mongodb package
sail composer require mongodb/laravel-mongodb
```

## (Optional) Add Redis service to Laravel sail

1. Add dependence for Redis service on ```docker-compose.yml```

```yml
laravel.test:
    # ...
    depends_on:
        - redis
    #...
```

2. Add Redis service on Docker container by adding following content on ```docker-compose.yml```

```yml
redis:
    image: 'redis:alpine'
    ports:
        - '${FORWARD_REDIS_PORT:-6379}:6379'
    volumes:
        - 'sailredis:/data'
    networks:
        - sail
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
```

3. Declare ```volumn``` on the same file

```yml
volumes:
    # ...
    sailredis:
        driver: local
    # ...

```

4. Rebuild by ```sail up -d```

Happy Coding!!!!!
