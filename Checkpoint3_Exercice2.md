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

1. Liste des disques et partitions existants :

```yaml
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
└─md0                    9:0    0     8G  0 raid1
  ├─md0p1              259:0    0 488,3M  0 part  /boot
  ├─md0p2              259:1    0     1K  0 part
  └─md0p5              259:2    0   7,5G  0 part
    ├─cp3--vg-root     253:0    0   2,8G  0 lvm   /
    └─cp3--vg-swap_1   253:1    0   976M  0 lvm   [SWAP]
sdc                      8:32   0     2G  0 disk
sr0                     11:0    1  1024M  0 rom
root@SRVLX01:/#
```

2. Création d'une partition LVM sur le nouveau disque (/dev/sdc) :

```yaml
root@SRVLX01:/# fdisk /dev/sdc

Bienvenue dans fdisk (util-linux 2.36.1).
Les modifications resteront en mémoire jusqu'à écriture.
Soyez prudent avant d'utiliser la commande d'écriture.

Le périphérique ne contient pas de table de partitions reconnue.
Création d'une nouvelle étiquette pour disque de type DOS avec identifiant de disque 0x9666ff4b.

Commande (m pour l'aide) : n
Type de partition
   p   primaire (0 primaire, 0 étendue, 4 libre)
   e   étendue (conteneur pour partitions logiques)
Sélectionnez (p par défaut) : p
Numéro de partition (1-4, 1 par défaut) :
Premier secteur (2048-4194303, 2048 par défaut) :
Dernier secteur, +/-secteurs ou +/-taille{K,M,G,T,P} (2048-4194303, 4194303 par défaut) :

Une nouvelle partition 1 de type « Linux » et de taille 2 GiB a été créée.

Commande (m pour l'aide) : t
Partition 1 sélectionnée
Code Hexa ou synonyme (taper L pour afficher tous les codes) :8e
Type de partition « Linux » modifié en « Linux LVM ».

Commande (m pour l'aide) : w
La table de partitions a été altérée.
Appel d'ioctl() pour relire la table de partitions.
Synchronisation des disques.

root@SRVLX01:/# 
```

3. Création du volume physique et du groupe de volumes LVM :

```yaml
root@SRVLX01:/# pvcreate /dev/sdc1
  Physical volume "/dev/sdc1" successfully created.
root@SRVLX01:/# vgcreate backup-vg /dev/sdc1
  Volume group "backup-vg" successfully created
root@SRVLX01:/#
```

4. Création d'un volume logique de 1,9 Go (en raison de l'espace légèrement inférieur à 2 Go disponible) :   

```yaml
root@SRVLX01:/# lvcreate -L 1.9G -n backup-lv backup-vg
  Rounding up size to full physical extent 1,90 GiB
  Logical volume "backup-lv" created.
root@SRVLX01:/#
```

5. Formatage du volume logique avec ext4 :

```yaml
root@SRVLX01:/# mkfs.ext4 /dev/backup-vg/backup-lv
mke2fs 1.46.2 (28-Feb-2021)
Creating filesystem with 498688 4k blocks and 124672 inodes
Filesystem UUID: e1d07495-26c0-4d81-9730-16289abc0208
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done

root@SRVLX01:/#
```

6. Montage du volume logique sur /var/lib/bareos/storage :

```yaml
root@SRVLX01:/# ls /var/lib/bareos/
bareos-dir.9101.state  bareos-dir.conmsg  bareos.sql  dbconfig-common  storage
root@SRVLX01:/# mount /dev/backup-vg/backup-lv /var/lib/bareos/storage
root@SRVLX01:/# 
```

7. Ajout de l'entrée dans /etc/fstab pour le montage automatique au démarrage :

Modification du fichier /etc/fstab en ajoutant la ligne :   

```
/dev/backup-vg/backup-lv /var/lib/bareos/storage ext4 defaults 0 2
```

<img width="468" alt="3_addvolfstab" src="https://github.com/user-attachments/assets/3beabb27-626a-4ad5-bbdc-30ef50b04894">    

8. Vérification du montage automatique :

```bash
root@SRVLX01:/# mount -a
root@SRVLX01:/# df -h /var/lib/bareos/storage
Sys. de fichiers                  Taille Utilisé Dispo Uti% Monté sur
/dev/mapper/backup--vg-backup--lv   1,9G     24K  1,8G   1% /var/lib/bareos/storage
root@SRVLX01:/#
```

```bash
root@SRVLX01:/# lsblk -f
NAME                      FSTYPE            FSVER    LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINT
sda
└─sda1                    linux_raid_member 1.2      cp3:0 32332561-cf16-c858-7035-17e881dd5c10
  └─md0
    ├─md0p1               ext2              1.0            9bba6d48-3e4b-42a6-bccc-12836de215ec    397,3M    10% /boot
    ├─md0p2
    └─md0p5               LVM2_member       LVM2 001       tlCGJ2-LG5u-kWGc-8kuO-wAiU-icBu-07BEcN
      ├─cp3--vg-root      ext4              1.0            bbc31a37-8e49-47fe-8fad-a3fe18919fdd        1G    56% /
      └─cp3--vg-swap_1    swap              1              8220bf51-2675-4203-91af-1c149f717652                  [SWAP]
sdb                       linux_raid_member 1.2      cp3:0 32332561-cf16-c858-7035-17e881dd5c10
└─md0
  ├─md0p1                 ext2              1.0            9bba6d48-3e4b-42a6-bccc-12836de215ec    397,3M    10% /boot
  ├─md0p2
  └─md0p5                 LVM2_member       LVM2 001       tlCGJ2-LG5u-kWGc-8kuO-wAiU-icBu-07BEcN
    ├─cp3--vg-root        ext4              1.0            bbc31a37-8e49-47fe-8fad-a3fe18919fdd        1G    56% /
    └─cp3--vg-swap_1      swap              1              8220bf51-2675-4203-91af-1c149f717652                  [SWAP]
sdc
└─sdc1                    LVM2_member       LVM2 001       6uR7wa-gOrN-WZPZ-50JE-Kylo-Pdto-8522s0
  └─backup--vg-backup--lv ext4              1.0            e1d07495-26c0-4d81-9730-16289abc0208      1,7G     0% /var/lib/bareos/storage
sr0
root@SRVLX01:/#
```

Le volume logique de 1,9 Go a été créé avec succès, formaté en ext4 et monté automatiquement sur /var/lib/bareos/storage au démarrage.

### Q.2.3.5

Pour vérifier l'espace disponible restant dans le groupe de volume, j'ai utilisé la commande suivante :

```bash
root@SRVLX01:/# vgs
```   
```bash
root@SRVLX01:/# vgs
  VG        #PV #LV #SN Attr   VSize  VFree
  backup-vg   1   1   0 wz--n- <2,00g 96,00m
  cp3-vg      1   2   0 wz--n-  7,51g <3,79g
root@SRVLX01:/#
```
**Il reste 96 Mo d'espace libre dans le groupe de volume backup-vg.**   


### Partie 4 : Sauvegardes

#### Q.2.4.1

Les trois composants Bareos installés sur la VM et leurs rôles respectifs sont :

**bareos-dir (Director)**  
Le `bareos-dir` est le chef d'orchestre de l'ensemble du système de sauvegarde. Il est responsable de la planification, du contrôle et du lancement des tâches de sauvegardes. Le Director coordonne les interactions entre les autres composants (`bareos-fd` et `bareos-sd`) pour effectuer les opérations de sauvegarde et de restauration.

**bareos-sd (Storage Daemon)**  
Le `bareos-sd` est chargé de gérer le stockage des données de sauvegarde. Il reçoit les données envoyées par le `bareos-fd` (File Daemon) et les écrit sur les supports de stockage configurés (disques, bandes magnétiques, etc.). Ce composant peut également être utilisé pour lire les données pendant les restaurations.

**bareos-fd (File Daemon)**  
Le `bareos-fd` est installé sur chaque machine dont les données doivent être sauvegardées. Il est responsable de la collecte des fichiers et des données à sauvegarder et de leur envoi au `bareos-sd` (Storage Daemon). Le File Daemon communique directement avec le `bareos-dir` pour exécuter les tâches de sauvegarde programmées.

En résumé :
- `bareos-dir` (Director) : Coordonne les sauvegardes et les restaurations.
- `bareos-sd` (Storage Daemon) : Gère l'écriture et la lecture des données sur les supports de stockage.
- `bareos-fd` (File Daemon) : Gère la collecte des fichiers à sauvegarder depuis les machines clientes.


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
La chaîne in_chain applique des règles de filtrage pour le trafic entrant (hook input).
La politique par défaut de cette chaîne est drop, ce qui signifie que tous les paquets entrants sont rejetés par défaut, sauf s'ils correspondent à l'une des règles suivantes :
- ct state established,related accept : Autorise les paquets qui font partie de connexions établies ou connexes.
- ct state invalid drop : Rejette les paquets dont l'état est invalide.
- iifname "lo" accept : Autorise le trafic sur l'interface de loopback lo (local).
- tcp dport 22 accept : Autorise les connexions TCP entrantes vers le port 22 (SSH).
- ip protocol icmp accept : Autorise les paquets ICMP (utilisés pour le ping et les messages d'erreur réseau).
- ip6 nexthdr ipv6-icmp accept : Autorise les paquets ICMPv6 (utilisés pour le ping et les messages d'erreur réseau en IPv6).

**Les règles actuellement appliquées sur Netfilter via nftables sont configurées pour bloquer par défaut tout le trafic entrant, sauf pour :**

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

### Q.2.5.4

Pour autoriser Bareos à communiquer avec les clients potentiels sur l'ensemble des machines du réseau local via `nftables`, il est nécessaire d'ajouter des règles qui permettent le trafic entrant et sortant sur les ports TCP 9101 à 9103, utilisés par Bareos pour ses communications.

#### Étapes pour ajouter les règles `nftables` :

1. **Afficher la table `nftables` existante :**

   Avant d'ajouter les règles, vérifions la table `nftables` existante :

   ```bash
   root@SRVLX01:/# nft list ruleset


