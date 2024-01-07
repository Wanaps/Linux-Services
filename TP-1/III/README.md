# III. Docker compose

Pour la fin de ce TP on va manipuler un peu `docker compose`.

🌞 **Créez un fichier `docker-compose.yml`**

- dans un nouveau dossier dédié `/home/<USER>/compose_test`

```bash
[user@localhost ~]$ mkdir compose_test && cd compose_test
```

- le contenu est le suivant :

```yml
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

Ce fichier est parfaitement équivalent à l'enchaînement de commandes suivantes (*ne les faites pas hein*, c'est juste pour expliquer) :

```bash
$ docker network create compose_test
$ docker run --name conteneur_nul --network compose_test debian sleep 9999
$ docker run --name conteneur_flopesque --network compose_test debian sleep 9999
```

🌞 **Lancez les deux conteneurs** avec `docker compose`

- déplacez-vous dans le dossier `compose_test` qui contient le fichier `docker-compose.yml`

```bash
[user@localhost ~]$ mkdir compose_test && cd compose_test
```

- go exécuter `docker compose up -d`

```bash
[user@localhost compose_test]$ docker compose up -d
[+] Running 3/3
 ✔ conteneur_flopesque Pulled                                                                                      2.4s
 ✔ conteneur_nul 1 layers [⣿]      0B/0B      Pulled                                                               2.4s
   ✔ bc0734b949dc Already exists                                                                                   0.0s
[+] Running 3/3
 ✔ Network compose_test_default                  Created                                                           0.4s
 ✔ Container compose_test-conteneur_flopesque-1  Started                                                           0.0s
 ✔ Container compose_test-conteneur_nul-1        Started                                                           0.0s
```


🌞 **Vérifier que les deux conteneurs tournent**

- toujours avec une commande `docker`

```bash
[user@localhost compose_test]$ dk ps
CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
5e4b10eafe1d   debian    "sleep 9999"   40 seconds ago   Up 39 seconds             compose_test-conteneur_flopesque-1
2235f3b3eeec   debian    "sleep 9999"   40 seconds ago   Up 39 seconds             compose_test-conteneur_nul-1
```

- tu peux aussi use des trucs comme `docker compose ps` ou `docker compose top` qui sont cools dukoo
  - `docker compose --help` pour voir les bails

<details closed><summary>en effet c'est koul!!!!!👍</summary>

```bash
[user@localhost compose_test]$ dk compose ps
NAME                                 IMAGE     COMMAND        SERVICE               CREATED          STATUS          PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   55 seconds ago   Up 55 seconds
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         55 seconds ago   Up 55 seconds
[user@localhost compose_test]$ dk compose top
compose_test-conteneur_flopesque-1
UID    PID     PPID    C    STIME   TTY   TIME       CMD
root   13340   13299   0    13:44   ?     00:00:00   sleep 9999

compose_test-conteneur_nul-1
UID    PID     PPID    C    STIME   TTY   TIME       CMD
root   13324   13281   0    13:44   ?     00:00:00   sleep 9999
```

</details>

🌞 **Pop un shell dans le conteneur `conteneur_nul`**

- référez-vous au mémo Docker
- effectuez un `ping conteneur_flopesque` (ouais ouais, avec ce nom là)
  - un conteneur est aussi léger que possible, aucun programme/fichier superflu : t'auras pas la commande `ping` !
  - il faudra installer un paquet qui fournit la commande `ping` pour pouvoir tester
  - juste pour te faire remarquer que les conteneurs ont pas besoin de connaître leurs IP : les noms fonctionnent

```bash
[user@localhost compose_test]$ dk exec -it compose_test-conteneur_nul-1 bash
root@4acae980bb4f:/# ping compose_test-conteneur_flopesque-1
bash: ping: command not found
```


```bash
root@4acae980bb4f:/# apt update
Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [52.1 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 Packages [8787 kB]
Get:5 http://deb.debian.org/debian bookworm-updates/main amd64 Packages [12.7 kB]
Get:6 http://deb.debian.org/debian-security bookworm-security/main amd64 Packages [134 kB]
Fetched 9185 kB in 1s (8175 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
root@4acae980bb4f:/# apt install iputils-ping
```

<details closed><summary>l'installation du package pour ping</summary>

```bash
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcap2-bin libpam-cap
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 96.2 kB of archives.
After this operation, 311 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://deb.debian.org/debian bookworm/main amd64 libcap2-bin amd64 1:2.66-4 [34.7 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 iputils-ping amd64 3:20221126-1 [47.1 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 libpam-cap amd64 1:2.66-4 [14.5 kB]
Fetched 96.2 kB in 0s (1090 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 6098 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.66-4_amd64.deb ...
Unpacking libcap2-bin (1:2.66-4) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20221126-1_amd64.deb ...
Unpacking iputils-ping (3:20221126-1) ...
Selecting previously unselected package libpam-cap:amd64.
Preparing to unpack .../libpam-cap_1%3a2.66-4_amd64.deb ...
Unpacking libpam-cap:amd64 (1:2.66-4) ...
Setting up libcap2-bin (1:2.66-4) ...
Setting up libpam-cap:amd64 (1:2.66-4) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gn
u/perl/5.36.0 /usr/local/share/perl/5.36.0 /usr/lib/x86_64-linux-gnu/perl5/5.36 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-g
nu/perl/5.36 /usr/share/perl/5.36 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20221126-1) ...
```

</details>

```bash
root@4acae980bb4f:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.083 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=2 ttl=64 time=0.107 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=3 ttl=64 time=0.097 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=4 ttl=64 time=0.076 ms
^C
--- conteneur_flopesque ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3088ms
rtt min/avg/max/mdev = 0.076/0.090/0.107/0.012 ms
```

ok premier degré c'est un truc de fou

![In the future](./img/in_the_future.jpg)
