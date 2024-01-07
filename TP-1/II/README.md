# II. Images

- [II. Images](#ii-images)
  - [1. Images publiques](#1-images-publiques)
  - [2. Construire une image](#2-construire-une-image)

## 1. Images publiques

🌞 **Récupérez des images**

nouveau jour, nouveau départ, on lance docker avant :
```bash
[user@wanaps ~]$ sudo systemctl start docker
```

- avec la commande `docker pull`
- récupérez :
  - l'image `python` officielle en version 3.11 (`python:3.11` pour la dernière version)
```bash
[user@wanaps ~]$ dk pull python:3.11
3.11: Pulling from library/python
bc0734b949dc: Already exists
b5de22c0f5cd: Already exists
917ee5330e73: Already exists
b43bd898d5fb: Already exists
7fad4bffde24: Already exists
1f68ce6a3e62: Pull complete
e27d998f416b: Pull complete
fefdcd9854bf: Pull complete
Digest: sha256:4e5e9b05dda9cf699084f20bb1d3463234446387fa0f7a45d90689c48e204c83
Status: Downloaded newer image for python:3.11
docker.io/library/python:3.11
```

  - l'image `mysql` officielle en version 5.7

```bash
[user@wanaps ~]$ dk pull mysql:5.7
5.7: Pulling from library/mysql
20e4dcae4c69: Pull complete
1c56c3d4ce74: Pull complete
e9f03a1c24ce: Pull complete
68c3898c2015: Pull complete
6b95a940e7b6: Pull complete
90986bb8de6e: Pull complete
ae71319cb779: Pull complete
ffc89e9dfd88: Pull complete
43d05e938198: Pull complete
064b2d298fba: Pull complete
df9a4d85569b: Pull complete
Digest: sha256:4bc6bc963e6d8443453676cae56536f4b8156d78bae03c0145cbe47c2aad73bb
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

  - l'image `wordpress` officielle en dernière version
    - c'est le tag `:latest` pour récupérer la dernière version
    - si aucun tag n'est précisé, `:latest` est automatiquement ajouté
```bash
[user@wanaps ~]$ dk pull wordpress
Using default tag: latest
latest: Pulling from library/wordpress
af107e978371: Already exists
6480d4ad61d2: Pull complete
95f5176ece8b: Pull complete
0ebe7ec824ca: Pull complete
673e01769ec9: Pull complete
74f0c50b3097: Pull complete
1a19a72eb529: Pull complete
50436df89cfb: Pull complete
8b616b90f7e6: Pull complete
df9d2e4043f8: Pull complete
d6236f3e94a1: Pull complete
59fa8b76a6b3: Pull complete
99eb3419cf60: Pull complete
22f5c20b545d: Pull complete
1f0d2c1603d0: Pull complete
4624824acfea: Pull complete
79c3af11cab5: Pull complete
e8d8239610fb: Pull complete
a1ff013e1d94: Pull complete
31076364071c: Pull complete
87728bbad961: Pull complete
Digest: sha256:ffabdfe91eefc08f9675fe0e0073b2ebffa8a62264358820bcf7319b6dc09611
Status: Downloaded newer image for wordpress:latest
docker.io/library/wordpress:latest
```

  - l'image `linuxserver/wikijs` en dernière version
    - ce n'est pas une image officielle car elle est hébergée par l'utilisateur `linuxserver` contrairement aux 3 précédentes
    - on doit donc avoir un moins haut niveau de confiance en cette image
```bash
[user@wanaps ~]$ dk pull linuxserver/wikijs
Using default tag: latest
latest: Pulling from linuxserver/wikijs
8b16ab80b9bd: Pull complete
07a0e16f7be1: Pull complete
145cda5894de: Pull complete
1a16fa4f6192: Pull complete
84d558be1106: Pull complete
4573be43bb06: Pull complete
20b23561c7ea: Pull complete
Digest: sha256:131d247ab257cc3de56232b75917d6f4e24e07c461c9481b0e7072ae8eba3071
Status: Downloaded newer image for linuxserver/wikijs:latest
docker.io/linuxserver/wikijs:latest
```
  
- listez les images que vous avez sur la machine avec une commande `docker`
```bash
[user@wanaps ~]$ dk images
REPOSITORY           TAG       IMAGE ID       CREATED       SIZE
debian               latest    2a033a8c6371   3 days ago    117MB
linuxserver/wikijs   latest    869729f6d3c5   6 days ago    441MB
mysql                5.7       5107333e08a8   9 days ago    501MB
wordpress            latest    fd2f5a0c6fba   2 weeks ago   739MB
python               3.11      22140cbb3b0c   2 weeks ago   1.01GB
nginx                latest    d453dd892d93   8 weeks ago   187MB
```


🌞 **Lancez un conteneur à partir de l'image Python**

- lancez un terminal `bash` ou `sh`
```bash
[user@wanaps ~]$ dk run -it python:3.11
Python 3.11.7 (main, Dec 19 2023, 20:33:49) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

- vérifiez que la commande `python` est installée dans la bonne version

```bash
Python 3.11.7 (main, Dec 19 2023, 20:33:49) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.version)
3.11.7 (main, Dec 19 2023, 20:33:49) [GCC 12.2.0]
>>>
```


## 2. Construire une image

Pour construire une image il faut :

- créer un fichier `Dockerfile`
- exécuter une commande `docker build` pour produire une image à partir du `Dockerfile`

🌞 **Ecrire un Dockerfile pour une image qui héberge une application Python**

- l'image doit contenir
  - une base debian (un `FROM`)
  - l'installation de Python (un `RUN` qui lance un `apt install`)
    - il faudra forcément `apt update` avant
    - en effet, le conteneur a été allégé au point d'enlever la liste locale des paquets dispos
    - donc nécessaire d'update avant de install quoique ce soit
  - l'installation de la librairie Python `emoji` (un `RUN` qui lance un `pip install`)
  - ajout de l'application (un `COPY`)
  - le lancement de l'application (un `ENTRYPOINT`)
- le code de l'application :

```python
import emoji

print(emoji.emojize("Cet exemple d'application est vraiment naze :thumbs_down:"))
```

- pour faire ça, créez un dossier `python_app_build`
  - pas n'importe où, c'est ton Dockerfile, ton caca, c'est dans ton homedir donc `/home/<USER>/python_app_build`
  - dedans, tu mets le code dans un fichier `app.py`
  - tu mets aussi `le Dockerfile` dedans

> *J'y tiens beaucoup à ça, comprenez que Docker c'est un truc que le user gère. Sauf si vous êtes un admin qui vous en servez pour faire des trucs d'admins, ça reste dans votre `/home`. Les dévs quand vous bosserez avec Windows, vous allez pas stocker vos machins dans `C:/Windows/System32/` si ? Mais plutôt dans `C:/Users/<TON_USER>/TonCaca/` non ? Alors pareil sous Linux please.*

🌞 **Build l'image**



- déplace-toi dans ton répertoire de build `cd python_app_build`

```bash
[user@localhost ~]$ cd python_app_build/
```

```bash
[user@localhost python_app_build]$ dk build . -t python_app:version_de_ouf
[+] Building 0.9s (12/12) FINISHED                                                                       docker:default
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 347B                                                                               0.0s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load metadata for docker.io/library/debian:latest                                                   0.7s
 => [1/7] FROM docker.io/library/debian@sha256:bac353db4cc04bc672b14029964e686cd7bad56fe34b51f432c1a1304b9928da    0.0s
 => [internal] load build context                                                                                  0.0s
 => => transferring context: 86B                                                                                   0.0s
 => CACHED [2/7] RUN apt update                                                                                    0.0s
 => CACHED [3/7] RUN apt install -y python3 python3-venv                                                           0.0s
 => CACHED [4/7] RUN python3 -m venv /venv                                                                         0.0s
 => CACHED [5/7] RUN /venv/bin/pip install emoji                                                                   0.0s
 => [6/7] WORKDIR /python_app_build/                                                                               0.0s
 => [7/7] COPY app.py /python_app_build/app.py                                                                     0.0s
 => exporting to image                                                                                             0.0s
 => => exporting layers                                                                                            0.0s
 => => writing image sha256:d317f164fb19cdc474e88ce61b847a8abc339f5892f43db1ab7610b55582f970                       0.0s
 => => naming to docker.io/library/python_app:version_de_ouf                                                       0.0s
```

- une fois le build terminé, constater que l'image est dispo avec une commande `docker`

```bash
[user@localhost python_app_build]$ dk images
REPOSITORY   TAG              IMAGE ID       CREATED              SIZE
python_app   version_de_ouf   4f33f85bb434   About a minute ago   218MB
```

🌞 **Lancer l'image**

- lance l'image avec `docker run` :

```bash
[user@localhost python_app_build]$ dk run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```
