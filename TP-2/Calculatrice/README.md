# Calculatrice Docker

Ce projet contient une calculatrice basique exécutée dans un conteneur Docker.

## Prérequis

Assurez-vous d'avoir Docker installé sur votre machine.

## Lancement du conteneur

1. Clonez ce dépôt sur votre machine :

    ```bash
    git clone git@github.com:Wanaps/Linux-Services.git
    ```

2. Accédez au répertoire du projet :

    ```bash
    cd Calculatrice-Docker
    ```

3. Construisez l'image Docker :

    ```bash
    docker build -t calculatrice .
    ```

4. Lancez le conteneur :

    ```bash
    docker run -d calculatrice
    ```
 