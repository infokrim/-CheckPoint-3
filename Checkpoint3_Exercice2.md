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

Il n'y a plus qu'à se connecter en SSH pour tester.   
