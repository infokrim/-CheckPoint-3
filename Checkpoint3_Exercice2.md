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
