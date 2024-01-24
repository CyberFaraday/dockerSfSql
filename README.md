# Docker & Symfony + PostgreSQL
Installation of an NginX / PostgreSQL / PHP8 / NodeJS / Yarn / Symfony CLI environment with Docker.
Additional services: PHPMyAdmin / MailDev
   

---  

## Installation  
  1. Clone the git repository :  
  `git clone https://github.com/CyberFaraday/dockerSfSql.git`
  or create a working directory with the name of your project,
  cd into this directory and enter the command:
  `git clone https://github.com/CyberFaraday/dockerSfSql.git .`

  2. Switch to the PostgreSQL branch:
  `git checkout postgresql`
 **do not switch to another branch from now on in this directory!**


  4. Duplicate the .env-sample file and rename it to .env :  
  `cd dockerSfSql` or cd into your custom project directory
  `cp .env-sample .env`  
  Edit this file by providing your informations...

  5. Mount Docker containers :  
  `docker compose up -d --build`
  It may take some time when first installing the required docker images. 

  7. when the previous operation is completed, to enter the main container (the php-node container) enter the command :  
  `docker compose exec php /bin/bash`

  8. For a simple PHP project, since the the Nginx server points to the "public" directory,
  so create a "public" directory in the "app" directory of your project working directory, this is where you will place your PHP files

  9. For a Symfony project, Test the Symfony CLI :  
  `symfony check:requirements`  

---  

## Install a Symfony project :  
**Note :To install a Symfony project, the "app" directory must be completely empty, so delete the delete-me.txt file and the possible "public" test folder if it is present:**
`rm delete-me.txt`
`rm -r public`
All commands are now executed in the terminal of your main container, i.e. the PHP-Node container :  
- `php bin/console […]`  
- `git […]`  
- `npm […]`  
- `yarn […]`  
 
- To use the LTS version of Symfony, cd into the "app" directory of your project,
and enter the command: `symfony new . --version=lts --webapp .`
this is just an example. 
