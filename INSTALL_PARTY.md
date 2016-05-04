# Installation de l'environnement Docker sur Windows

## Prés-requis
- Ajouter les droits d'administration à l'utilisateur
- Ajouter au besoin les variables d'environnement du proxy
```HTTP_PROXY http://proxy.recif.nc:80```

## Installer [Docker Toolbox](https://www.docker.com/products/docker-toolbox)
- Lancer l'utilitaire d'installation I:\Infrastructures\BASD\Applications\docker\DockerToolbox-1.10.3.exe (téléchargée [ici](https://github.com/docker/toolbox/releases/download/v1.10.3/DockerToolbox-1.10.3.exe))
- Activer au besoin l'installation de virtualbox (doit être en version 5)

## Installer [Babun](http://babun.github.io)
- Copier et dézipper en local le zip I:\Infrastructures\BASD\Applications\docker\babun-1.2.0-dist.zip (téléchargée [ici](http://projects.reficio.org/babun/download))
- lancer l'installation

## Configurer Babun
- Editer le fichier de configuration Babun ```vim ~/.babunrc```
- Ajouter les variables d'environnement de proxy ```export http_proxy=http://proxy.recif.nc:80
export https_proxy=$http_proxy
export ftp_proxy=$http_proxy
export no_proxy=localhost```
- Modifier les user agent
```export USER_AGENT="Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.111 Safari/537.36"```
- Charger la configuration ```source ~/.babunrc```
- Vérifier l'installation ```babun check```
- Mettre à jour Babun ```babun update```

## Activer le copier/coller par sélection souris
- Lancer la commande ```echo "set mouse-=a" >> ~/.vimrc```

## Activer [TTY](http://www.linusakesson.net/programming/tty/) sur Babun avec le [workaround de tiangolo](https://github.com/tiangolo/babun-docker)
- Lancer la commande ```curl -s https://raw.githubusercontent.com/tiangolo/babun-docker/master/setup.sh | source /dev/stdin```

## Ajouter des [alias](https://github.com/tcnksm/docker-alias) pour les commandes docker
- Pour zsh lancer la commande ```curl -fsSL https://raw.github.com/tcnksm/docker-alias/master/zshrc >> ~/.zshrc && source ~/.zshrc```
- Pour bash lancer la commande ```curl -fsSL https://raw.github.com/tcnksm/docker-alias/master/zshrc >> ~/.bashrc && source ~/.bashrc```
  
## Créer un Docker Host avec docker machine
- Afficher les docker host :
    ```docker-machine ls```
- Supprimer au besoin la machine default
    ```docker-machine rm default```
- Créer une machine default avec variables de proxy : 
    ```docker-machine create -d virtualbox --engine-env HTTP_PROXY=http://proxy.recif.nc:80 --engine-env HTTPS_PROXY=http://proxy.recif.nc:80 default```
- Afficher les docker hosts 
    ```docker-machine ls```
- Afficher les variables d'environnement à positionner pour communiquer avec le default host
    ```docker-machine env (default)```
- Positionner les variables d'environnement pour communiquer avec l'engine de default host
    ```eval $(docker-machine env)```
- Ajouter la variable NO_PROXY pour exclure l'IP du docker host
    ```export NO_PROXY=$(echo $DOCKER_HOST | sed 's/tcp:\/\/\([^:]*\).*/\1/')```
    ```export no_proxy=$(echo $DOCKER_HOST | sed 's/tcp:\/\/\([^:]*\).*/\1/')```
