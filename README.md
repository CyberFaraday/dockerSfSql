# Docker & Symfony + MySQL 
Installation of an NginX / MySQL / PHP8 / NodeJS / Yarn / Symfony CLI environment with Docker.
Additional services: PHPMyAdmin / MailDev
   

---  

## Installation  
  1. Clone the git repository :  
  `git clone https://github.com/CyberFaraday/dockerSfSql.git`
  or create a working directory with the name of your project,
  cd into this directory and enter the command:
  `git clone https://github.com/CyberFaraday/dockerSfSql.git .`

  2. for a MySql based environment, stay on the main branch which is the same as the mysql branch, or switch to the mysql branch:
  `git checkout mysql`

  4. Duplicate the .env-sample file and rename it to .env :  
  `cd dockerSfSql` or cd into your custom project directory
  `cp .env-sample .env`  
  Edit this file by providing your informations

  5. Mount Docker containers :  
  `docker compose up -d --build`
  It may take some time when first installing the required docker images. 

  7. quand la précédente  opération est terminée, pour entrer dans le containeur principal (le container php-node) entrez la commande :  
  `docker compose exec php /bin/bash`  

  8. Tester le Symfony CLI :  
  `symfony check:requirements`  

---  

## Installer un projet Symfony :  
Toutes les commandes sont exécutées dans le terminal de votre conteneur principal :  
- `php bin/console […]`  
- `git […]`  
- `npm […]`  
- `yarn […]`  

**Note : supprimer le fichier delete-me.txt avant d'initialiser un projet SF**  
`rm delete-me.txt`  

- Latest : `symfony new --full .`  
- LTS : `composer create-project symfony/website-skeleton:"^5.4" .`  
