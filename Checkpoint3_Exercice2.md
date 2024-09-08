### Partie 1 : Gestion des utilisateurs

#### Q.2.1.1

Pour créer un compte utilisateur sur le serveur, j'ai suivi les étapes suivantes :

1. **Connexion en tant que superutilisateur (root)** :

```bash
su
```
2. **Création de l'utilisateur karim :** 
   
```bash
adduser karim
```

3. **Commandes et sorties** :

```bash
wilder@SRVLX01:~$ su
Mot de passe :
root@SRVLX01:/home/wilder# cd /
root@SRVLX01:/# adduser karim
Ajout de l'utilisateur « karim » ...
Ajout du nouveau groupe « karim » (1001) ...
Ajout du nouvel utilisateur « karim » (1001) avec le groupe « karim » ...
Création du répertoire personnel « /home/karim »...
Copie des fichiers depuis « /etc/skel »...
Nouveau mot de passe :
Retapez le nouveau mot de passe :
passwd: password updated successfully
Changing the user information for karim
Enter the new value, or press ENTER for the default
        Full Name []: Karim
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Cette information est-elle correcte ? [O/n]o
root@SRVLX01:/# ^C
```

#### Q.2.1.2

Voici les préconisations de sécurisation pour le compte utilisateur sur le serveur :

- **Utiliser SSHv2** : S'assurer que le protocole SSHv1 est désactivé.
- **Interdire l'accès SSH au compte root** : Modifier la configuration SSH pour `PermitRootLogin no`.
- **Créer un utilisateur par personne ayant besoin d'un accès** : Limiter l'accès à des comptes spécifiques avec `AllowUsers`.
- **Changer le port par défaut de SSH** : Utiliser un port non standard pour SSH.
- **Désactiver le X11Forwarding** : Désactiver cette option dans la configuration SSH.
- **Désactiver les connexions sans mot de passe** : Configurer `PermitEmptyPasswords no`.
- **Réduire la durée maximale d'authentification** : Configurer `LoginGraceTime 30`.
- **Utiliser des clés RSA de 2048 bits minimum** : Générer des clés SSH sécurisées.
- **Activer l'option StrictModes** : Vérifier les permissions des fichiers SSH.

Ces mesures permettront de renforcer significativement la sécurité du serveur. Ces conseils sont tirés d'un article concernant la sécurisation SSH. 


### Partie 2 : Configuration de SSH

#### Q.2.2.1

Pour désactiver complètement l'accès à distance de l'utilisateur root, j'ai modifié le fichier de configuration SSH comme suit :

1. **Ouvrir le fichier de configuration SSH :**

```bash
nano /etc/ssh/sshd_config
```

2. ** Ajout de la ligne suivante pour interdire l'accès SSH au compte root :**

```bash
PermitRootLogin no
```
<img width="330" alt="Q311_norootconnect" src="https://github.com/user-attachments/assets/52461b57-6830-42b9-acae-d05212aa1f98">

sauvegarder et quitter Nano : 

Appuyez sur Ctrl + O, puis appuyez sur la touche Entrée pour enregistrer les modifications.

Appuyez sur Ctrl + X pour quitter l'éditeur de texte nano.


2. ** Redémarrage du service SSH :**

```bash
systemctl restart sshd
```
3. **Commandes et sorties** :

```bash
root@SRVLX01:/# nano /etc/ssh/sshd_config
root@SRVLX01:/# systemctl restart sshd
root@SRVLX01:/
```



NB : Bonne pratique, sauvegarder le fichier par défaut avant de le modifier  (ex `cp /etc/ssh/sshd_config /etc/ssh/sshd_config.old`).    

#### Q.2.2.2

Pour autoriser l'accès à distance uniquement à mon compte personnel `karim`, j'ai modifié le fichier de configuration SSH comme suit :

1. **Ouvrir le fichier de configuration SSH :**

```bash
nano /etc/ssh/sshd_config
```

<img width="352" alt="Q311a_norootconnect" src="https://github.com/user-attachments/assets/5949c983-7267-48c1-a99d-6b9d32a72afc">

sauvegarder et quitter Nano : 

Appuyez sur Ctrl + O, puis appuyez sur la touche Entrée pour enregistrer les modifications.

Appuyez sur Ctrl + X pour quitter l'éditeur de texte nano.

2. ** Redémarrage du service SSH :**

```bash
systemctl restart sshd
```

3. **Commandes et sorties** :

```bash
root@SRVLX01:/# nano /etc/ssh/sshd_config
root@SRVLX01:/# systemctl restart sshd
root@SRVLX01:/
```

#### Q.2.2.3

Pour mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe, j'ai suivi les étapes suivantes :

1. **Génération de la paire de clés SSH sur la machine hôte (Windows 11) avec Powershell ou wsl :**


```bash
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): /mnt/c/Users/Karim/.ssh/id_rsa
/mnt/c/Users/Karim/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /mnt/c/Users/Karim/.ssh/id_rsa
Your public key has been saved in /mnt/c/Users/Karim/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xC59toA8OlpYVaZjaUnCsMN14hgcoZ7v4EaJuT6j8I0 root@KRIMPC
The key's randomart image is:
+---[RSA 3072]----+
| .==+ o o        |
| o.*.= B         |
|. = . O o        |
|. .. = *         |
|.+. . = S o      |
|oo.o . o + .     |
|oo..+     .      |
|+=o= .           |
|=oE..            |
+----[SHA256]-----+
root@KRIMPC:/mnt/c/Users/Karim/.ssh#
```


<img width="869" alt="Q312_createkeypub" src="https://github.com/user-attachments/assets/10d4e763-1c8e-4b5f-997e-934e840bd3e5">

2. **Copie de la clé publique sur le serveur Linux (VM SRVLX01) à l'aide de WSL :**

```bash
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ls -la
total 8
drwxrwxrwx 1 karim karim 4096 Sep  6 19:50 .
drwxrwxrwx 1 karim karim 4096 Sep  6 03:37 ..
-rwxrwxrwx 1 karim karim 1876 Sep  6 19:50 id_rsa
-rwxrwxrwx 1 karim karim  395 Sep  6 19:50 id_rsa.pub
-rwxrwxrwx 1 karim karim  849 Sep  6 00:07 known_hosts
-rwxrwxrwx 1 karim karim   99 Sep  6 00:07 known_hosts.old
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ssh
ssh          ssh-add      ssh-agent    ssh-argv0    ssh-copy-id  ssh-keygen   ssh-keyscan
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ssh-copy-id karim@192.168.1.200
/usr/bin/ssh-copy-id: ERROR: No identities found
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ssh-copy-id -i /mnt/c/Users/Karim/.ssh/id_rsa.pub karim@192.168.1.200
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/mnt/c/Users/Karim/.ssh/id_rsa.pub"
The authenticity of host '192.168.1.200 (192.168.1.200)' can't be established.
ED25519 key fingerprint is SHA256:oxgpSguGlIJPOa9k7uA+dnijLMNZhQ1nEUn4gJroNFE.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
karim@192.168.1.200's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'karim@192.168.1.200'"
and check to make sure that only the key(s) you wanted were added.

root@KRIMPC:/mnt/c/Users/Karim/
```

<img width="864" alt="Q213_copysshkey" src="https://github.com/user-attachments/assets/05ff32f4-6644-474a-9002-341b3ed9c4e9">    

3. **Ouvrir le fichier de configuration SSH sur le serveur pour désactiver l'authentification par mot de passe :**

```
root@SRVLX01:/home/karim# nano /etc/ssh/sshd_config
root@SRVLX01:/home/karim#
```
- Trouver les lignes avec les commandes suivantes enlever le # et modifier par yes ou no si necéssaire.   

<img width="338" alt="Q312b_keypub" src="https://github.com/user-attachments/assets/fd08a8fd-8057-4a21-a181-9f711828ac46">

- Comparaison du contenu des clés sur WSL et le servuer debian :      

```bash
root@KRIMPC:/mnt/c/Users/Karim/.ssh# ls
id_rsa  id_rsa.pub  known_hosts  known_hosts.old
root@KRIMPC:/mnt/c/Users/Karim/.ssh# cat id_rsa.pub
ssh-rsa "série de caractères de la clé" karim@KRIMPC
root@KRIMPC:/mnt/c/Users/Karim/.ssh#

root@SRVLX01:/home/karim/.ssh# cat authorized_keys
ssh-rsa "série de caractères de la clé" karim@KRIMPC
root@SRVLX01:/home/karim/.ssh# ls -la /home/karim/.ssh
total 12
drwx------ 2 karim karim 4096  7 sept. 18:18 .
drwxr-xr-x 3 karim karim 4096  7 sept. 18:18 ..
-rw------- 1 karim karim  395  7 sept. 18:18 authorized_keys
root@SRVLX01:/home/karim/.ssh# 
```

<img width="861" alt="Q223b" src="https://github.com/user-attachments/assets/d0a0e042-ef45-48ea-8fed-f950b5cd27d3">    

<img width="588" alt="Q223c" src="https://github.com/user-attachments/assets/d4d949da-2f46-4877-8e5e-2977eb4d55dd">    



Configuration MobaXterm pour utilisation clé privé :    

- Cocher l'option "Use private key"   

- Sélectionner la Clé Privée   

<img width="428" alt="Q223d_mobax" src="https://github.com/user-attachments/assets/b451a9e8-13ac-4f40-92cf-3ce77d9b1933">    

Il n'y a plus qu'à se connecter en SSH.   

### Partie 3 : Analyse du stockage

#### Q.2.3.1

Liste des systèmes de fichiers montés :   
Exécuter la commande suivante  :   

```bash
cat /proc/mounts
```

```bash
root@SRVLX01:/home/karim# cat /proc/mounts
sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
udev /dev devtmpfs rw,nosuid,relatime,size=480536k,nr_inodes=120134,mode=755 0 0
devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,nosuid,nodev,noexec,relatime,size=100012k,mode=755 0 0
/dev/mapper/cp3--vg-root / ext4 rw,relatime,errors=remount-ro 0 0
securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /dev/shm tmpfs rw,nosuid,nodev 0 0
tmpfs /run/lock tmpfs rw,nosuid,nodev,noexec,relatime,size=5120k 0 0
cgroup2 /sys/fs/cgroup cgroup2 rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot 0 0
pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
none /sys/fs/bpf bpf rw,nosuid,nodev,noexec,relatime,mode=700 0 0
systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=30,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=10671 0 0
mqueue /dev/mqueue mqueue rw,nosuid,nodev,noexec,relatime 0 0
tracefs /sys/kernel/tracing tracefs rw,nosuid,nodev,noexec,relatime 0 0
hugetlbfs /dev/hugepages hugetlbfs rw,relatime,pagesize=2M 0 0
debugfs /sys/kernel/debug debugfs rw,nosuid,nodev,noexec,relatime 0 0
fusectl /sys/fs/fuse/connections fusectl rw,nosuid,nodev,noexec,relatime 0 0
configfs /sys/kernel/config configfs rw,nosuid,nodev,noexec,relatime 0 0
/dev/md0p1 /boot ext2 rw,relatime 0 0
tmpfs /run/user/1000 tmpfs rw,nosuid,nodev,relatime,size=100012k,nr_inodes=25003,mode=700,uid=1000,gid=1000 0 0
tmpfs /run/user/1001 tmpfs rw,nosuid,nodev,relatime,size=100012k,nr_inodes=25003,mode=700,uid=1001,gid=1001 0 0
root@SRVLX01:/home/karim# df -h
Sys. de fichiers         Taille Utilisé Dispo Uti% Monté sur
udev                       470M       0  470M   0% /dev
tmpfs                       98M    656K   98M   1% /run
/dev/mapper/cp3--vg-root   2,7G    1,5G  1,1G  60% /
tmpfs                      489M     16K  489M   1% /dev/shm
tmpfs                      5,0M       0  5,0M   0% /run/lock
/dev/md0p1                 471M     49M  398M  11% /boot
tmpfs                       98M       0   98M   0% /run/user/1000
tmpfs                       98M       0   98M   0% /run/user/1001
root@SRVLX01:/home/karim#
```

`cat /proc/mounts`, qui montre des détails techniques complets.    
`df -h` fournit une vue plus lisible de l'utilisation de l'espace disque


#### Q.2.3.2

Pour déterminer le type de systèmes de stockage utilisés et les types de systèmes de fichiers associés, utiliser la commande suivante :   

```bash
lsblk -f
```

```bash
root@SRVLX01:/# lsblk -f
NAME                   FSTYPE            FSVER    LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINT
sda
└─sda1                 linux_raid_member 1.2      cp3:0 32332561-cf16-c858-7035-17e881dd5c10
  └─md0
    ├─md0p1            ext2              1.0            9bba6d48-3e4b-42a6-bccc-12836de215ec    397,3M    10% /boot
    ├─md0p2
    └─md0p5            LVM2_member       LVM2 001       tlCGJ2-LG5u-kWGc-8kuO-wAiU-icBu-07BEcN
      ├─cp3--vg-root   ext4              1.0            bbc31a37-8e49-47fe-8fad-a3fe18919fdd        1G    56% /
      └─cp3--vg-swap_1 swap              1              8220bf51-2675-4203-91af-1c149f717652                  [SWAP]
sr0
```

Le serveur utilise un système de stockage RAID1 pour la redondance des données (linux_raid_member).    
LVM (Logical Volume Manager) est utilisé pour la gestion des volumes logiques, ce qui permet une gestion flexible de l'espace de stockage.    
Les types de systèmes de fichiers sont :    
ext2 pour la partition /boot.   
ext4 pour la partition racine /.   
swap pour la gestion de la mémoire virtuelle.     


  
#### Q.2.3.3

### Étapes à Suivre :

1. **Ajouter un nouveau disque de 8,00 Gio au serveur.**    

- Accéder aux paramètres de votre machine virtuelle :    
  - Ouvrir VirtualBox et sélectionner la VM `Checkpoint3-SRVLX01`.    
  - Cliquer sur le bouton **Paramètres**.    
   - Accéder à l'onglet **Stockage**.    
   - Ajouter un nouveau disque en cliquant sur l'icône de disque dur avec le symbole **+**.
 
     
<img width="717" alt="1_addhdd" src="https://github.com/user-attachments/assets/25d00bdf-1c0d-44a0-923e-50ced0ed3bc5">    

2. **Vérification de la présence du nouveau disque dur :**

- Exécuter la commande suivante sur le serveur pour vérifier que le nouveau disque est détecté :

```bash
 root@SRVLX01:/# lsblk
```bash

root@SRVLX01:/# lsblk
NAME                   MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                      8:0    0     8G  0 disk
└─sda1                   8:1    0     8G  0 part
  └─md0                  9:0    0     8G  0 raid1
    ├─md0p1            259:0    0 488,3M  0 part  /boot
    ├─md0p2            259:1    0     1K  0 part
    └─md0p5            259:2    0   7,5G  0 part
      ├─cp3--vg-root   253:0    0   2,8G  0 lvm   /
      └─cp3--vg-swap_1 253:1    0   976M  0 lvm   [SWAP]
sdb                      8:16   0     8G  0 disk
sr0                     11:0    1  1024M  0 rom
root@SRVLX01:/#
```

On voit le nouveau disque sdb de 8 Go.    

3. Vérification de l'état du RAID

La commande suivante permet de vérifier l'état actuel de tous les ensembles RAID sur le système :     

```bash
cat /proc/mdstat
```


```yaml
root@SRVLX01:/# cat /proc/mdstat
Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10]
md0 : active raid1 sda1[0]
      8381440 blocks super 1.2 [2/1] [U_]

unused devices: <none>
root@SRVLX01:/#
```

[2/1] indique qu'il y a 2 disques dans le RAID, mais un seul est actuellement actif.   
[U_] montre que le premier disque (sda1) est actif (U), tandis que le second est manquant (_).     

4. Vérification des détails du RAID
   
Pour obtenir des informations détaillées sur l'ensemble RAID md0, utilisez la commande :    

```bash
mdadm --detail /dev/md0
```

```yaml
root@SRVLX01:/# mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Tue Dec 20 10:02:28 2022
        Raid Level : raid1
        Array Size : 8381440 (7.99 GiB 8.58 GB)
     Used Dev Size : 8381440 (7.99 GiB 8.58 GB)
      Raid Devices : 2
     Total Devices : 1
       Persistence : Superblock is persistent

       Update Time : Sun Sep  8 02:25:38 2024
             State : clean, degraded
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

              Name : cp3:0
              UUID : 32332561:cf16c858:703517e8:81dd5c10
            Events : 13486

    Number   Major   Minor   RaidDevice State
       0       8        1        0      active sync   /dev/sda1
       -       0        0        1      removed
root@SRVLX01:/# 
```

Cette sortie confirme que l'ensemble RAID md0 est en état "dégradé" (degraded), avec un seul disque actif (sda1).    

Aucun dispositif de secours (spare) n'est présent.    

D'où la demande d'ajout d'un disque de taille égale à sda1


4. Ajout du nouveau disque sdb au RAID

Pour ajouter le nouveau disque (sdb) au volume RAID md0, utilisez la commande :    

```bash
mdadm --manage /dev/md0 --add /dev/sdb
```

```yaml
root@SRVLX01:/# mdadm --manage /dev/md0 --add /dev/sdb
mdadm: added /dev/sdb
root@SRVLX01:/#
```

5. Surveillance la reconstruction du RAID

 la commande suivante permet de surveiller la reconstruction du RAID en temps réel :    

 <img width="593" alt="2_watchreconstruction" src="https://github.com/user-attachments/assets/de3e5e78-a44d-4a4d-86a2-e06ae8565e1a">    


```bash
mdadm --manage /dev/md0 --add /dev/sdb
```


```yaml
root@SRVLX01:/# watch cat /proc/mdstat
Toutes les 2,0s: cat /proc/mdstat                                                                                SRVLX01: Sun Sep  8 02:51:12 2024

Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10]
md0 : active raid1 sdb[2] sda1[0]
      8381440 blocks super 1.2 [2/2] [UU]

unused devices: <none>
```

[2/2] [UU] indique que les deux disques du RAID sont désormais actifs et synchronisés.    


#### Q.2.3.4   

Pour ajouter un nouveau volume logique LVM de 2 Gio pour héberger des sauvegardes et le monter automatiquement à chaque démarrage dans l'emplacement par défaut /var/lib/bareos/storage, j'ai suivi les étapes suivantes :    

1. Vérifier l'espace disponible dans le groupe de volumes (VG) :

```yaml
root@SRVLX01:/# vgdisplay
  --- Volume group ---
  VG Name               cp3-vg
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               7,51 GiB
  PE Size               4,00 MiB
  Total PE              1923
  Alloc PE / Size       953 / 3,72 GiB
  Free  PE / Size       970 / <3,79 GiB
  VG UUID               BMardR-vLO6-CToa-ad0f-XVh0-ODeS-cX7Obt

root@SRVLX01:/#
```
** Free  PE / Size       970 / <3,79 GiB **  

La sortie montre qu'il y a environ 3,79 Go d'espace libre dans le VG cp3-vg.    

2. Lister les volumes logiques existants pour choisir un nom approprié :

```yaml
root@SRVLX01:/# lvs
  LV     VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cp3-vg -wi-ao----  <2,77g
  swap_1 cp3-vg -wi-ao---- 976,00m
root@SRVLX01:/# lvdisplay cp3-vg
  --- Logical volume ---
  LV Path                /dev/cp3-vg/root
  LV Name                root
  VG Name                cp3-vg
  LV UUID                kveJwG-MIiG-Rnyh-jFaD-S4tK-1z7G-jPtXq4
  LV Write Access        read/write
  LV Creation host, time cp3, 2022-12-20 10:04:20 +0100
  LV Status              available
  # open                 1
  LV Size                <2,77 GiB
  Current LE             709
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Path                /dev/cp3-vg/swap_1
  LV Name                swap_1
  VG Name                cp3-vg
  LV UUID                xGf2tz-vfjS-OXIy-uKxI-Ww2o-n5CO-j6v0WW
  LV Write Access        read/write
  LV Creation host, time cp3, 2022-12-20 10:04:20 +0100
  LV Status              available
  # open                 2
  LV Size                976,00 MiB
  Current LE             244
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1

root@SRVLX01:/#
```
Cette commande montre qu'il y a deux volumes logiques existants : root et swap_1.

3. Créer un nouveau volume logique nommé bareos_storage :

J'ai créé un nouveau volume logique de 2 Gio nommé bareos_storage dans le VG cp3-vg avec la commande :   

```yaml
lvcreate -n bareos_storage -L 2G cp3-vg
```
```yaml
root@SRVLX01:/# lvcreate -n bareos_storage -L 2G cp3-vg
  Logical volume "bareos_storage" created.
root@SRVLX01:/# lvs
  LV             VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  bareos_storage cp3-vg -wi-ao----   2,00g
  root           cp3-vg -wi-ao----  <2,77g
  swap_1         cp3-vg -wi-ao---- 976,00m
root@SRVLX01:/#
```   
Après vérification, le nouveau volume logique est bien crée.   

4. Formater le volume logique avec ext4 :  

```yaml
root@SRVLX01:/# mkfs.ext4 /dev/cp3-vg/bareos_storage
mke2fs 1.46.2 (28-Feb-2021)
Creating filesystem with 524288 4k blocks and 131072 inodes
Filesystem UUID: 39b7b091-62cd-4128-a5ff-940c421e1496
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

root@SRVLX01:/#
```



5. Vérifier la présence du répertoire de destination et monter le volume logique :

J'ai vérifié que le répertoire /var/lib/bareos/storage existait déjà, puis j'ai monté le volume logique dans ce répertoire :

```yaml
root@SRVLX01:/# ls /var/lib/bareos/
bareos-dir.9101.state  bareos-dir.conmsg  bareos.sql  dbconfig-common  storage
root@SRVLX01:/# mount /dev/cp3-vg/bareos_storage /var/lib/bareos/storage
root@SRVLX01:/# df -h /var/lib/bareos/storage
Sys. de fichiers                   Taille Utilisé Dispo Uti% Monté sur
/dev/mapper/cp3--vg-bareos_storage   2,0G     24K  1,8G   1% /var/lib/bareos/storage
root@SRVLX01:/# 
```

6. Vérifier que le volume est monté correctement :

```yaml
root@SRVLX01:/# df -h /var/lib/bareos/storage
Sys. de fichiers                   Taille Utilisé Dispo Uti% Monté sur
/dev/mapper/cp3--vg-bareos_storage   2,0G     24K  1,8G   1% /var/lib/bareos/storage
root@SRVLX01:/#
```
La sortie montre que le volume est monté avec succès.   

7. Ajouter le montage au fichier /etc/fstab pour le montage automatique au démarrage et vérifier la configuration de /etc/fstab en testant le montage :

J'ai exécuté la commande suivante pour vérifier que tout était c

Pour assurer le montage automatique du volume au démarrage, j'ai ajouté l'entrée correspondante dans le fichier /etc/fstab :    

```
root@SRVLX01:/# echo '/dev/cp3-vg/bareos_storage /var/lib/bareos/storage ext4 defaults 0 2' >> /etc/fstab
root@SRVLX01:/# mount -a
root@SRVLX01:/# 
```
Aucune erreur n'a été retournée, confirmant que la configuration est correcte.   

Ayant déjà eu des ennuis de corruption de fichier fstab dans le passé, j'ai quand même tenu à vérifier le contenu du fichier fstab même si le test de montage automatique s'est effectué avec succès.

```bash
root@SRVLX01:/# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# systemd generates mount units based on this file, see systemd.mount(5).
# Please run 'systemctl daemon-reload' after making changes here.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/cp3--vg-root /               ext4    errors=remount-ro 0       1
# /boot was on /dev/md0p1 during installation
UUID=9bba6d48-3e4b-42a6-bccc-12836de215ec /boot           ext2    defaults        0       2
/dev/mapper/cp3--vg-swap_1 none            swap    sw              0       0
/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
/dev/cp3-vg/bareos_storage /var/lib/bareos/storage ext4 defaults 0 2
root@SRVLX01:/#
```
Le contenu est correct.    

### Q.2.3.5

Pour vérifier l'espace disponible restant dans le groupe de volume, j'ai utilisé la commande suivante :

```bash
root@SRVLX01:/# vgs
```   
```bash
root@SRVLX01:/# vgs
  VG     #PV #LV #SN Attr   VSize VFree
  cp3-vg   1   3   0 wz--n- 7,51g <1,79g
root@SRVLX01:/#
```
- VG Name (Nom du VG) : cp3-vg
- #LV : 3 volumes logiques sont présents dans ce VG.
- VSize (Taille totale du VG) : 7,51 GiB.
- VFree (Espace libre dans le VG) : <1,79 GiB.

**Cela montre qu'il reste environ 1,79 Go d'espace libre dans le groupe de volumes cp3-vg après la création du nouveau volume logique bareos_storage.**   


### Partie 4 : Sauvegardes

#### Q.2.4.1

Les trois composants Bareos installés sur la VM et leurs rôles respectifs sont :

**bareos-dir (Director)**  
Le `bareos-dir` Coordonne les sauvegardes et restaurations, planifie et contrôle les tâches.    

**bareos-sd (Storage Daemon)**  
Le `bareos-sd` Gère le stockage des données de sauvegarde sur les supports configurés.    

**bareos-fd (File Daemon)**  
Le `bareos-fd` Collecte les fichiers à sauvegarder depuis les machines et les envoie au Storage Daemon.    


### Partie 5 : Filtrage et analyse réseau   

### Q.2.5.1

Pour afficher les règles actuellement appliquées sur Netfilter, j'ai utilisé la commande suivante :

```bash
root@SRVLX01:/# nft list ruleset
```

```bash
root@SRVLX01:/# nft list ruleset
table inet inet_filter_table {
        chain in_chain {
                type filter hook input priority filter; policy drop;
                ct state established,related accept
                ct state invalid drop
                iifname "lo" accept
                tcp dport 22 accept
                ip protocol icmp accept
                ip6 nexthdr ipv6-icmp accept
        }
}
root@SRVLX01:/#
```
- Table inet_filter_table :
Cette table gère les paquets pour les protocoles IPv4 et IPv6.

- Chaîne in_chain :
Filtrage du trafic entrant avec une politique par défaut drop.
- ct state established,related accept : Autorise les paquets qui font partie de connexions établies ou connexes.
- ct state invalid drop : Rejette les paquets dont l'état est invalide.
- iifname "lo" accept : Autorise le trafic sur l'interface de loopback lo (local).
- tcp dport 22 accept : Autorise les connexions TCP entrantes vers le port 22 (SSH).
- ip protocol icmp accept : Autorise les paquets ICMP.
- ip6 nexthdr ipv6-icmp accept : Autorise les paquets ICMPv6 .

**Les règles appliquées sur Netfilter via nftables bloquent par défaut tout le trafic entrant, sauf pour :**

**- Les connexions établies et connexes**
**- Les paquets ICMP et ICMPv6**
**- Le trafic sur le port SSH (port 22)**
**- Le trafic sur l'interface locale (loopback)**    

### Q.2.5.2   

Les types de communications autorisées par les règles `nftables` actuelles sont :
- Les connexions établies et connexes,
- Le trafic sur l'interface locale `lo` (loopback),
- Les connexions SSH (port 22),
- Les paquets ICMP (pour IPv4) et ICMPv6 (pour IPv6).

Toutes les autres communications sont bloquées par défaut.

### Q.2.5.3  

Les types de communications interdits par les règles `nftables` actuelles incluent :
- Tout le trafic entrant qui ne correspond pas aux règles d'autorisation explicites.
- Les paquets avec un état invalide.
- Toutes les connexions TCP autres que celles vers le port 22 (SSH).
- Tout autre protocole ou type de communication non explicitement autorisé (comme UDP ou d'autres ports TCP).

Ces règles assurent que seul le trafic explicitement autorisé est permis, bloquant ainsi tout le reste pour renforcer la sécurité du système.

### Q.2.5.4 Ajouter les règles nécessaires pour autoriser Bareos

Pour autoriser Bareos à communiquer avec les clients sur le réseau local en utilisant les ports TCP 9101 à 9103, voici les étapes que j'ai suivies :  
 

- Vérifier les règles appliquées :
J'ai utilisé la commande suivante pour lister les règles nftables actuellement appliquées :

```bash
nft list ruleset
```
  
- Faire une copie de sauvegarde du fichier de configuration :
Avant de modifier quoi que ce soit, j'ai créé une copie de sauvegarde du fichier de configuration :


```bash
cp /etc/nftables.conf /etc/nftables.conf.old
```

- Ajouter les Règles pour Bareos :

```bash
nft add rule inet inet_filter_table in_chain tcp dport {9101, 9102, 9103} accept
```

- Vérifier que les Règles sont Appliquées Correctement :

```bash
nft list ruleset
```

- Connexion à l'adresse IP locale (127.0.0.1) sur les ports 9102 et 9103 :

Les commandes telnet 127.0.0.1 9102 et telnet 127.0.0.1 9103 ont également réussi à établir une connexion sur les ports 9102 et 9103 respectivement.
Les messages "Connected to 127.0.0.1" indiquent que le serveur Bareos accepte les connexions sur ces ports en local.
 

```bash
root@SRVLX01:/#  telnet 192.168.1.200 9101
Trying 192.168.1.200...
Connected to 192.168.1.200.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/# telnet 127.0.0.1 9102
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/# telnet 127.0.0.1 9103
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/#
```
Les tests de connectivité confirment que :

Les règles nftables autorisent bien le trafic sur les ports 9101, 9102, et 9103, permettant ainsi à Bareos de communiquer correctement.

```bash
root@SRVLX01:/# cp /etc/nftables.conf /etc/nftables.conf.old
root@SRVLX01:/# nft flush ruleset
root@SRVLX01:/# nft add rule inet inet_filter_table in_chain tcp dport {9101, 9102, 9103} accept
root@SRVLX01:/# nft -f /etc/nftables.conf
root@SRVLX01:/# nft list ruleset
table inet inet_filter_table {
        chain in_chain {
                type filter hook input priority filter; policy drop;
                ct state established,related accept
                ct state invalid drop
                iifname "lo" accept
                tcp dport 22 accept
                ip protocol icmp accept
                ip6 nexthdr ipv6-icmp accept
                tcp dport { 9101, 9102, 9103 } accept
        }
}
root@SRVLX01:/# cat /etc/nftables.conf
table inet inet_filter_table {
        chain in_chain {
                type filter hook input priority filter; policy drop;
                ct state established,related accept
                ct state invalid drop
                iifname "lo" accept
                tcp dport 22 accept
                ip protocol icmp accept
                ip6 nexthdr ipv6-icmp accept
                tcp dport { 9101, 9102, 9103 } accept
        }
}
root@SRVLX01:/# telnet <adresse_IP_client> 9101
bash: adresse_IP_client: Aucun fichier ou dossier de ce type
root@SRVLX01:/#  telnet 192.168.1.200 9101
Trying 192.168.1.200...
Connected to 192.168.1.200.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/# telnet 127.0.0.1 9102
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/# telnet 127.0.0.1 9103
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]
▒Connection closed by foreign host.
root@SRVLX01:/#

nft add rule inet inet_filter_table in_chain tcp dport {9101, 9102, 9103} accept
```
