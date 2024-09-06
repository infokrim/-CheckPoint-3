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

Ces mesures permettront de renforcer significativement la sécurité du serveur.    


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

