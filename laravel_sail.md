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

3. Run ```docker-compose.yml``` which was created to the root directory of the project.

4. Modify your ```.env``` file with environment variables as the application needs.

5. Build the docker image by following command:

```bash
./vendor/bin/sail up -d
```

6. Access ```http://locahost``` on browser, you will see the application is running.

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

Happy Coding!!!!!