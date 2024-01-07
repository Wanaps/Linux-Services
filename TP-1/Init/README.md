# I. Init

- [I. Init](#i-init)
  - [1. Installation de Docker](#1-installation-de-docker)
  - [2. Vérifier que Docker est bien là](#2-vérifier-que-docker-est-bien-là)
  - [3. sudo c pa bo](#3-sudo-c-pa-bo)
  - [4. Un premier conteneur en vif](#4-un-premier-conteneur-en-vif)
  - [5. Un deuxième conteneur en vif](#5-un-deuxième-conteneur-en-vif)

## 1. Installation de Docker

Pour installer Docker, il faut **toujours** (comme d'hab en fait) se référer à la doc officielle.

**Je vous laisse donc suivre les instructions de la doc officielle pour installer Docker dans la VM.**

> ***Il n'y a pas d'instructions spécifiques pour Rocky dans la doc officielle**, mais rocky est très proche de CentOS. Vous pouvez donc suivre les instructions pour CentOS 9.*

## 2. Vérifier que Docker est bien là

```bash
# est-ce que le service Docker existe ?
systemctl status docker

# si oui, on le démarre alors
sudo systemctl start docker

# voyons si on peut taper une commande docker
sudo docker info
sudo docker ps
```

## 3. sudo c pa bo

🌞 **Ajouter votre utilisateur au groupe `docker`**
```bash
[user@wanaps ~]$ sudo groupadd docker
[sudo] password for user:
groupadd: group 'docker' already exists
```

j'ai ajouté un alias pour plus de simplicité :
```bash
alias dk="docker"
```

## 4. Un premier conteneur en vif

> *Je rappelle qu'un "conteneur" c'est juste un mot fashion pour dire qu'on lance un processus un peu isolé sur la machine.*

Bon trève de blabla, on va lancer un truc qui juste marche.

On va lancer un conteneur NGINX qui juste fonctionne, puis custom un peu sa conf. Ce serait par exemple pour tester une conf NGINX, ou faire tourner un serveur NGINX de production.

> *Hé les dévs, **jouez le jeu bordel**. NGINX c'est pas votre pote OK, mais on s'en fout, c'est une app comme toutes les autres, comme ta chatroom ou ta calculette. Ou Netflix ou LoL ou Spotify ou un malware. NGINX il est réputé et standard, c'est juste un outil d'étude pour nous là. Faut bien que je vous fasse lancer un truc. C'est du HTTP, c'est full standard, vous le connaissez, et c'est facile à tester/consommer : avec un navigateur.*

🌞 **Lancer un conteneur NGINX**

- avec la commande suivante :

```bash
docker run -d -p 9999:80 nginx
```
```bash
[user@wanaps ~]$ dk run -d -p 9999:80 nginx
4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d
[user@wanaps ~]$
```

🌞 **Visitons**

- vérifier que le conteneur est actif avec une commande qui liste les conteneurs en cours de fonctionnement
```bash
[user@wanaps ~]$ dk ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
4e9a7c5a2050   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:9999->80/tcp, :::9999->80/tcp   festive_germain
[user@wanaps ~]$
```
- afficher les logs du conteneur
``` bash
[user@wanaps ~]$ dk logs 4e9
```

<details closed><summary>résultat entier (c'est pour éviter un gros pavé)</summary>

```bash
[user@wanaps ~]$ dk logs 4e9
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/12/21 10:21:13 [notice] 1#1: using the "epoll" event method
2023/12/21 10:21:13 [notice] 1#1: nginx/1.25.3
2023/12/21 10:21:13 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2023/12/21 10:21:13 [notice] 1#1: OS: Linux 5.14.0-284.30.1.el9_2.x86_64
2023/12/21 10:21:13 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2023/12/21 10:21:13 [notice] 1#1: start worker processes
2023/12/21 10:21:13 [notice] 1#1: start worker process 28
[user@wanaps ~]$
```

</details>

---
- afficher toutes les informations relatives au conteneur avec une commande `docker inspect

```bash
[user@wanaps ~]$ dk inspect 4e9
```

<details closed><summary>pareil c'est un bordel monstre</summary>

```bash
[user@wanaps ~]$ dk inspect 4e9
[
    {
        "Id": "4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d",
        "Created": "2023-12-21T10:21:12.965827471Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 49021,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-12-21T10:21:13.20285724Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d453dd892d9357f3559b967478ae9cbc417b52de66b53142f6c16c8a275486b9",
        "ResolvConfPath": "/var/lib/docker/containers/4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d/hostname",
        "HostsPath": "/var/lib/docker/containers/4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d/hosts",
        "LogPath": "/var/lib/docker/containers/4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d/4e9a7c5a2050a6e34044af7e5d0bdeece9e10111a7cf6311b3a0700b593e5f9d-json.log",
        "Name": "/festive_germain",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "9999"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                12,
                119
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/df861d15ced3b564961f1310c1a30d1024a45a54777f67c9c9c7b64b87ce2a1b-init/diff:/var/lib/docker/overlay2/2bae310e2a2ab11bf69535354a14c7df4c0e5f6a9e17c3fa066223424e809c11/diff:/var/lib/docker/overlay2/13c214b93a60e7d0cdf672a5ed1640f32f4626f38db0c5c08aa00d17931449db/diff:/var/lib/docker/overlay2/edd0e6ce5d069b87e4662fd0b02084830e811a9a4b66bff671b6cff6850f1907/diff:/var/lib/docker/overlay2/3af8fb98444bc9c9ca7b8edce8a3943c5e8a11962aa655fd231327ef27f20939/diff:/var/lib/docker/overlay2/2970bc29962fdf8cd2f66fdc284f48b41f2ceab0cdd2feb6112d36e7ce82228a/diff:/var/lib/docker/overlay2/752d9d4bf59b0dfbbd690082301ec3a269503a8a7b90c029333f99f19f59e9f4/diff:/var/lib/docker/overlay2/f43ce722a06111621d93bc303c7a8cacbe3971afde5b55ce6c2112aac882d3dd/diff",
                "MergedDir": "/var/lib/docker/overlay2/df861d15ced3b564961f1310c1a30d1024a45a54777f67c9c9c7b64b87ce2a1b/merged",
                "UpperDir": "/var/lib/docker/overlay2/df861d15ced3b564961f1310c1a30d1024a45a54777f67c9c9c7b64b87ce2a1b/diff",
                "WorkDir": "/var/lib/docker/overlay2/df861d15ced3b564961f1310c1a30d1024a45a54777f67c9c9c7b64b87ce2a1b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "4e9a7c5a2050",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.25.3",
                "NJS_VERSION=0.8.2",
                "PKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "79f1c569efffded2ded83d7527d6baf84ac00fb51c66782683ec363cb3a1fb01",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "9999"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "9999"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/79f1c569efff",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "8eff7ed6b2531bdce0a53ab98276e8d66e73c075b9c4ccfbd1355ce25be06607",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "c710058da8fa2cbf1ee5a565abe967538018f2ac8c9d4bfa8b669e1d7cac8d07",
                    "EndpointID": "8eff7ed6b2531bdce0a53ab98276e8d66e73c075b9c4ccfbd1355ce25be06607",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

</details>

---

- afficher le port en écoute sur la VM avec un `sudo ss -lnpt`

```bash
[user@wanaps ~]$ sudo ss -lnpt
[sudo] password for user:
State         Recv-Q        Send-Q               Local Address:Port                Peer Address:Port        Process
LISTEN        0             4096                       0.0.0.0:9999                     0.0.0.0:*            users:(("docker-proxy",pid=48981,fd=4))
LISTEN        0             128                        0.0.0.0:22                       0.0.0.0:*            users:(("sshd",pid=22968,fd=3))
LISTEN        0             4096                          [::]:9999                        [::]:*            users:(("docker-proxy",pid=48986,fd=4))
LISTEN        0             128                           [::]:22                          [::]:*            users:(("sshd",pid=22968,fd=4))
```

---
- ouvrir le port `9999/tcp` (vu dans le `ss` au dessus normalement) dans le firewall de la VM
```bash
[user@wanaps ~]$ sudo firewall-cmd --add-port=9999/tcp --permanent
success
```
---

- depuis le navigateur de votre PC, visiter le site web sur `http://IP_VM:9999`
ip de la vm :
```bash
[user@wanaps ~]$ ip a
[...]
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e0:76:60 brd ff:ff:ff:ff:ff:ff
    inet 10.1.2.3/24 brd 10.1.2.255 scope global dynamic noprefixroute enp0s8
[...]
```


🌞 **On va ajouter un site Web au conteneur NGINX**

- créez un dossier `nginx`
  - pas n'importe où, c'est ta conf caca, c'est dans ton homedir donc `/home/<TON_USER>/nginx/`

```bash
[user@wanaps nginx]$ mkdir /home/user/nginx/ && cd /home/user/nginx
```

- dedans, deux fichiers : `index.html` (un site nul) `site_nul.conf` (la conf NGINX de notre site nul)
```bash
[user@wanaps nginx]$ echo "<h1>salut mec</h1>" >> index.html
```
(en vrai c'est un gain de temps énorme de temps de faire ça je suis un génie un peu)

- config NGINX minimale pour servir un nouveau site web dans `site_nul.conf` :

```bash
[user@wanaps nginx]$ nano site_nul.conf
[user@wanaps nginx]$ cat site_nul.conf
server {
        listen          8080;

        location / {
                root var/www/html;
        }
}
```
(ouais bon je vais pas trop faire le fou non plus)

- lancez le conteneur avec la commande en dessous, notez que :
  - on partage désormais le port 8080 du conteneur (puisqu'on l'indique dans la conf qu'il doit écouter sur le port 8080)
  - on précise les chemins des fichiers en entier
  - note la syntaxe du `-v` : à gauche le fichier à partager depuis ta machine, à droite l'endroit où le déposer dans le conteneur, séparés par le caractère `:`
  - c'est long putain comme commande

```bash
[user@wanaps nginx]$ dk run -d -p 9999:8080 -v /home/user/nginx/index.html:/var/www/html/index.html -v /home/user/nginx
/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
c2b3389fff2a7eec112877a3a2f05819b746baf8e685d9c27234335058985c49
```

🌞 **Visitons**

- vérifier que le conteneur est actif
- aucun port firewall à ouvrir : on écoute toujours port 9999 sur la machine hôte (la VM)
- visiter le site web depuis votre PC

## 5. Un deuxième conteneur en vif

🌞 **Lance un conteneur Python, avec un shell**

- il faut indiquer au conteneur qu'on veut lancer un shell
- un shell c'est "interactif" : on saisit des trucs (input) et ça nous affiche des trucs (output)
  - il faut le préciser dans la commande `docker run` avec `-it`
- ça donne donc :

```bash
docker run -it python bash
```

🌞 **Installe des libs Python**

- installez deux libs, elles ont été choisies complètement au hasard (avec la commande `pip install`):
  - `aiohttp`
  - `aioconsole`

```bash
root@778df5f569ae:/# pip install aiohttp aioconsole
[...]
```

- tapez la commande `python` pour ouvrir un interpréteur Python
```bash
root@778df5f569ae:/# python
Python 3.12.1 (main, Dec 19 2023, 20:14:15) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

- taper la ligne `import aiohttp` pour vérifier que vous avez bien téléchargé la lib
```python
>>> import aiohttp
>>>
```
c'est GOUDE

➜ **Tant que t'as un shell dans un conteneur**, tu peux en profiter pour te balader. Tu peux notamment remarquer :

- si tu fais des `ls` un peu partout, que le conteneur a sa propre arborescence de fichiers
```bash
root@778df5f569ae:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

- si t'essaies d'utiliser des commandes usuelles un poil évoluées, elles sont pas là
  - genre t'as pas `ip a` ou ce genre de trucs

    ```bash
    root@778df5f569ae:/# ip a
    bash: ip: command not found
    ```
    :'(