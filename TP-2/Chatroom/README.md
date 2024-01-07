# Chatroom Docker Container

Ce projet contient un conteneur Docker pour exécuter une chatroom.

## Prérequis

Assurez-vous d'avoir Docker installé sur votre machine.

## Lancement du conteneur

1. Clonez ce dépôt sur votre machine :

    ```bash
    git clone git@github.com:Wanaps/Linux-Services.git
    ```

2. Accédez au répertoire du projet :

    ```bash
    cd Chatroom
    ```

3. Construisez l'image Docker :

    ```bash
    docker build -t chatroom .
    ```

4. Lancez le conteneur :

    ```bash
    docker run chatroom
    ```

## Configuration

Le conteneur utilise le port 8080 par défaut. Si vous souhaitez utiliser un autre port, modifiez le paramètre `-p` lors de la commande `docker run`.
