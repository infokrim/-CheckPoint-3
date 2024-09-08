# Exercice 1 : Manipulations pratiques sur VM Windows    

## Partie 1 : Gestion des utilisateurs    

**Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs de société que Kelly Rhameur.**    

1. **Copier l'utilisateur existant :**    

- Cliquer avec le bouton droit sur **Kelly Rhameur** dans la console **Active Directory Users and Computers** et sélectionner **Copy**.     

     
<img width="599" alt="Q111a-createuser" src="https://github.com/user-attachments/assets/4546d295-f282-4b36-a03f-4a1806f69a35">    

2. **Créer le nouvel utilisateur :**    

- Remplissez les champs :    

  - **First Name** : Lionel    

  - **Last Name** : Lemarchand    

  - **User Logon Name** : Lionel.Lemarchand    

  - Cliquez sur **Next**.   


<img width="215" alt="Q111b-createuser" src="https://github.com/user-attachments/assets/14be31f5-affd-4d36-bdb2-cf249ed3b922">     


3. **Définir le mot de passe :**   

- Choisissez un mot de passe et cochez les options appropriées (par exemple, "User must change password at next logon").    

- Cliquez sur **Next**.    


<img width="217" alt="Q111c-createuser" src="https://github.com/user-attachments/assets/bf0c3cba-21a1-4f30-9504-ea8dd88b8cfe">       


<img width="218" alt="Q111d-createuser" src="https://github.com/user-attachments/assets/2ee4f5cc-8c0e-42f6-bd72-5b5136122202">     


4. **Finaliser la création de l'utilisateur :**

- Cliquez sur **Finish** pour terminer.   


<img width="218" alt="Q111e-createuser" src="https://github.com/user-attachments/assets/c230872c-b5b9-475e-8c11-3ee4b747bba3">     


5. **Vérification et Modification des Attributs Utilisateur :**      


1. J'ai modifié les informations dans l'onglet **General** :    

- Ajout de l'adresse e-mail : `Lionel.Lemarchand@TSSR.LAN`.    
   
<img width="410" alt="Q111f-CUchangeproperties" src="https://github.com/user-attachments/assets/8ed671ce-d169-4b6d-94cb-1d30394c2839">    
 
2. J'ai mis à jour le **Job Title** dans l'onglet **Organization** :     

- Titre de l'emploi changé en "Directeur des Ressources Humaines".     
     
<img width="408" alt="Q111h-CUchangeproperties" src="https://github.com/user-attachments/assets/9e98c6b4-24d5-40be-890f-d074d8c8df9b">    


**Q.1.1.2 Créer une OU DeactivatedUsers et déplacer le compte désactivé de Kelly Rhameur dedans.**    


1. **Création de l'OU DeactivatedUsers :**    

- J'ai ouvert la console **Active Directory Users and Computers**.    

- J'ai cliqué avec le bouton droit sur le domaine **TSSR.LAN** et sélectionné **New > Organizational Unit**.    


<img width="595" alt="Q112a_createOU" src="https://github.com/user-attachments/assets/b5f3ed5a-f4a7-4dbc-949b-2411a8d0a48c">     


2. **Nommer la nouvelle OU :**   

- J'ai nommé l'unité d'organisation **DeactivatedUsers** et j'ai coché l'option **Protect container from accidental deletion** pour protéger l'OU contre toute suppression accidentelle.    

- J'ai cliqué sur **OK** pour créer l'OU.    


<img width="217" alt="Q112a1_createOU" src="https://github.com/user-attachments/assets/962f7ce4-98cc-413d-8d36-410695ed916d">     

3. **Vérification de la création de l'OU :**    

- J'ai vérifié que l'OU **DeactivatedUsers** a été correctement créée dans la liste des unités d'organisation sous le domaine **TSSR.LAN**.     

<img width="598" alt="Q112a2_createOU" src="https://github.com/user-attachments/assets/6fbc2f09-61f5-4b5e-bbfb-d6502d4cc769">    


4. **Désactivation et déplacement du compte de Kelly Rhameur :**    

- J'ai localisé l'utilisateur **Kelly Rhameur** dans **Active Directory Users and Computers**.   

- J'ai cliqué avec le bouton droit sur **Kelly Rhameur** et sélectionné **Disable Account** pour désactiver le compte.   


<img width="598" alt="Q112b3_move" src="https://github.com/user-attachments/assets/250cd345-d026-4276-82f6-b30bec814a6d">


- Ensuite, j'ai cliqué avec le bouton droit sur **Kelly Rhameur** et sélectionné **Move...** pour déplacer le compte.

- J'ai sélectionné l'OU **DeactivatedUsers** dans la fenêtre de sélection et j'ai cliqué sur **OK**.


<img width="159" alt="Q112b4_move" src="https://github.com/user-attachments/assets/d7ba9ee3-15b3-4035-a659-84585db242d3">    



5. **Vérification du déplacement du compte :**    
 
- J'ai vérifié que le compte de **Kelly Rhameur** a bien été déplacé dans l'OU **DeactivatedUsers**.

<img width="597" alt="Q112b5_moved" src="https://github.com/user-attachments/assets/2ef24156-892c-498c-a694-fac7605798e2">    


**Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.**    

1. **Accès aux propriétés de Kelly Rhameur dans la nouvelle OU :**   

- Je suis allé(e) dans l'OU **DeactivatedUsers** où **Kelly Rhameur** a été déplacée.    

- J'ai cliqué avec le bouton droit sur **Kelly Rhameur** et sélectionné **Properties**.    

<img width="598" alt="Q113b_modOU" src="https://github.com/user-attachments/assets/c70c2644-3f71-45aa-8b7f-02a8db8e49c3">   

 
2. **Suppression des groupes de l'ancienne OU :**    

- Dans l'onglet **Member Of**, j'ai sélectionné le groupe lié à l'ancienne OU (**GrpUsersDirectionDesRessourcesHumaines**).    

- J'ai cliqué sur **Remove** pour retirer **Kelly Rhameur** de ce groupe.    


<img width="205" alt="Q113c_remove" src="https://github.com/user-attachments/assets/0bb427a1-59e0-4093-bcaf-dda97e668281">     

<img width="191" alt="Q113d_remove" src="https://github.com/user-attachments/assets/a8db01ba-a30d-44ef-8325-95c980b6401a">     


   
4. **Enregistrement des modifications :**    

- J'ai cliqué sur **Apply** puis sur **OK** pour sauvegarder les changements.     

<img width="206" alt="Q113e_apply" src="https://github.com/user-attachments/assets/84dfda61-900a-47d8-a322-d863c46e597a">    

<img width="204" alt="Q113f_ok" src="https://github.com/user-attachments/assets/e20ee761-18d2-458f-9298-11c72b2a002b">     


## Q.1.1.4 Créer le dossier individuel du nouvel utilisateur et archiver celui de Kelly Rhameur en le suffixant par "-ARCHIVE".     

### 1. Création du dossier pour Lionel Lemarchand    

- J'ai localisé le dossier *DossiersIndividuels** à l'emplacement local **F:\DossiersIndividuels**.   

<img width="561" alt="Q114a_inddir" src="https://github.com/user-attachments/assets/146fc972-3351-40bb-9ccb-bc18e76086c1">    


- J'ai créé un nouveau dossier en faisant un clic droit dans l'emplacement de stockage, puis en sélectionnant **New > Folder**.    


<img width="597" alt="Q114b_newdir" src="https://github.com/user-attachments/assets/318828c3-9440-4805-a740-fdedd1e903cf">    


- J'ai nommé le nouveau dossier **lionel.lemarchand**.     

<img width="518" alt="Q114c_newfold" src="https://github.com/user-attachments/assets/7f0aa454-0854-4ef9-860b-298f91bbc671">    


- J'ai configuré les permissions pour que Lionel Lemarchand ait les droits appropriés :     

- J'ai fait un clic droit sur le dossier **lionel.lemarchand** et sélectionné **Properties**.    


<img width="559" alt="Q114d_propert" src="https://github.com/user-attachments/assets/51df06ba-be3e-47a0-817d-844998d29f04">     



- Sous l'onglet **Security**, j'ai cliqué sur **Edit...** puis sur **Add...** pour ajouter **Lionel Lemarchand**.    


<img width="182" alt="Q114e_sec" src="https://github.com/user-attachments/assets/bc69ff1c-8427-4663-86af-726c6173407e">    

<img width="189" alt="Q114f_sec" src="https://github.com/user-attachments/assets/e4f9e51f-d3cf-415b-ae3f-1a78f7925537">     


- J'ai sélectionné **Lionel Lemarchand** et configuré les permissions en cochant **Read & Execute**, **List Folder Contents**, **Read**, et **Write**.    


<img width="189" alt="Q114f_sec" src="https://github.com/user-attachments/assets/1636ebf9-eb1d-4756-8944-c33ab923449e">     

<img width="180" alt="Q114g_add" src="https://github.com/user-attachments/assets/81271309-4e56-421b-a42f-5c45fa739c69">    

<img width="228" alt="Q114h_checkname" src="https://github.com/user-attachments/assets/a3d39043-2b4a-446d-b05c-36f8628fc475">     

<img width="179" alt="Q114i_droits" src="https://github.com/user-attachments/assets/9d84bbff-611a-4b49-9f2f-591962f439b8">     

<img width="180" alt="Q114j_droitsok" src="https://github.com/user-attachments/assets/c45676ee-e801-4873-89f2-ad340582edf1">     



### 2. Archivage du dossier de Kelly Rhameur     

- J'ai localisé le dossier **kelly.rhameur** existant sous **F:\DossiersIndividuels**.     


<img width="565" alt="Q114k_archivekelly" src="https://github.com/user-attachments/assets/50ffa7b0-48a7-4c2e-b3ba-dd2e9d7b03ff">     


- J'ai fait un clic droit sur **kelly.rhameur** et sélectionné **Rename** pour le renommer en **kelly.rhameur-ARCHIVE**.     

<img width="562" alt="Q114l_suffixe" src="https://github.com/user-attachments/assets/06446543-6288-4123-b794-59cd1e4eeaa0">    

### Partie 2 : Restriction utilisateurs     

**Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.**      

1. Accéder aux propriétés de l'utilisateur Gabriel Ghul :    

- J'ai recherché l'utilisateur Gabriel Ghul dans Active Directory Users and Computers.    

<img width="257" alt="1_findGab" src="https://github.com/user-attachments/assets/3d2b035c-db63-408c-a8d7-278f72f9cd17">     


- Ensuite, j'ai fait un clic droit sur Gabriel Ghul et sélectionné **Properties**.     

<img width="296" alt="2_goproperties" src="https://github.com/user-attachments/assets/570ca604-1894-4779-a015-26c39012f7a2">     



2. Définir les heures de connexion autorisées :     

- Dans l'onglet **Account** des propriétés de l'utilisateur, j'ai cliqué sur le bouton **Logon Hours...**.     



- J'ai modifié les heures de connexion en cochant les cases pour autoriser la connexion uniquement du lundi au vendredi, de 7h à 17h, et cliqué sur **OK** pour valider.     


<img width="252" alt="4_modtime" src="https://github.com/user-attachments/assets/70b26543-882d-4e9e-ac67-a0542d8821d6">    


3. Appliquer les modifications :    

- Cliquez sur **Apply** puis sur **OK** pour sauvegarder les changements.    


**Q.1.2.2 Bloquer la connexion de l'utilisateur au seul ordinateur CLIENT01.**     

1. Définir les postes de travail autorisés :     

- Toujours dans l'onglet **Account** des propriétés de l'utilisateur, cliquez sur le bouton **Log On To...**.    


<img width="204" alt="3_gabprop" src="https://github.com/user-attachments/assets/6362ef97-4efd-481c-97a7-9c5e0234b7ec">     


- Dans la fenêtre **Logon Workstations**, sélectionnez **The following computers** et entrez le nom de l'ordinateur **CLIENT01**.    

- Cliquez sur **Add** puis sur **OK** pour valider.     

<img width="183" alt="5_addpc" src="https://github.com/user-attachments/assets/590d5212-8de1-46ed-93b9-a7759fedec49">     
 

2. Appliquer les modifications :     

- Cliquez sur **Apply** puis sur **OK** pour sauvegarder les changements.     

<img width="205" alt="6_apply" src="https://github.com/user-attachments/assets/1cac50fe-a644-4347-b770-0709cf4bcef0">       


## Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers     

1. **Ouvrir le Gestionnaire de Stratégie de Groupe (Group Policy Management) :**     

- Ouvrez le **Server Manager** et allez dans **Tools > Group Policy Management**.    

<img width="525" alt="7_GPO" src="https://github.com/user-attachments/assets/1f31bce2-9265-48cc-baa6-eb23bac2d1fc">     


2. **Créer une nouvelle GPO :**    

- Dans l'arborescence du domaine **TSSR.LAN**, faites un clic droit sur l'OU **LabUsers** et sélectionnez **Create a GPO in this domain, and Link it here...**.     

<img width="432" alt="7a_gpo" src="https://github.com/user-attachments/assets/41e7ec3b-7f64-4df6-8b46-a9a989bfc20a">     


- Nommez la nouvelle GPO **Password Policy for LabUsers** et cliquez sur **OK**.     
  
<img width="194" alt="8_newGPO" src="https://github.com/user-attachments/assets/8107e32f-e661-4631-a28a-af381ec1dc5e">     


3. **Configurer la GPO pour la stratégie de mot de passe :**     


- Faites un clic droit sur la nouvelle GPO **Password Policy for LabUsers** et sélectionnez **Edit**.     


<img width="624" alt="9_editGPO" src="https://github.com/user-attachments/assets/66c7ccce-a0a4-42b4-b89e-da42050bf626">     



- Dans l'éditeur de stratégie de groupe, naviguez vers **Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy**.    


<img width="392" alt="10_PasswordPolicy" src="https://github.com/user-attachments/assets/6baa855c-93c8-49d3-abdb-59565474049d">    

<img width="392" alt="11_confstarteg" src="https://github.com/user-attachments/assets/587eb3a4-345b-4a3d-92e1-12d0b74c1c35">     


4. **Définir les paramètres de la stratégie de mot de passe :**      


- **Enforce password history** : Double-cliquez sur cette option, cochez **Define this policy setting** et définissez **5** mots de passe à se souvenir.    

<img width="208" alt="12_passhist" src="https://github.com/user-attachments/assets/bcd263a0-59b9-4064-b5f6-14cac9eaebff">     





- **Maximum password age** : Double-cliquez sur cette option, cochez **Define this policy setting** et définissez l'âge maximal du mot de passe à **30 jours**.    


<img width="208" alt="13_passpropr" src="https://github.com/user-attachments/assets/f2925884-a2a9-47b7-be5b-9af4bf2bafe1">     


- **Minimum password age** : Double-cliquez sur cette option, cochez **Define this policy setting** et définissez l'âge minimal du mot de passe à **5 jours**.    


<img width="207" alt="15_ageminipass" src="https://github.com/user-attachments/assets/3e566923-2f6d-4c1b-868d-96b9b1ac6231">     


- **Minimum password length** : Double-cliquez sur cette option, cochez **Define this policy setting** et définissez la longueur minimale du mot de passe à **12 caractères**.    

<img width="206" alt="16_passlenght" src="https://github.com/user-attachments/assets/3fd3f8a1-49e4-4dd2-adb3-da2559525978">     



- **Password must meet complexity requirements** : Double-cliquez sur cette option, cochez **Define this policy setting** et sélectionnez **Enabled**.     


<img width="207" alt="17_passcomplex" src="https://github.com/user-attachments/assets/8ec8a6dd-6fed-4446-9df5-364233c79875">     


5. **Appliquer et propager les modifications :**     

- Fermez l'éditeur de stratégie de groupe et forcez une mise à jour de la stratégie de groupe en utilisant la commande `gpupdate /force` dans l'invite de commande.    

<img width="489" alt="18_cmdupd" src="https://github.com/user-attachments/assets/c1b91f59-7769-43d8-bd32-6704ef3291c4">     


6. **Vérifier l'application de la stratégie :**     

- Pour vérifier que la GPO est appliquée correctement, ouvrez une invite de commande sur un client du domaine et exécutez la commande `gpresult /r`.     

<img width="348" alt="19_gpresult1" src="https://github.com/user-attachments/assets/e1d5cf8b-5ccc-4e82-99c9-6303c60486ff">     

<img width="349" alt="20_gpresult2" src="https://github.com/user-attachments/assets/0b47e6f9-c9b8-4aef-a53c-1ba8e87f22e2">     


<img width="349" alt="21_gpresult3" src="https://github.com/user-attachments/assets/24bc901a-9c75-4e83-bc82-959bda4d162e">     



Avec ces étapes et les captures d'écran fournies, la stratégie de mot de passe sera correctement configurée et appliquée aux utilisateurs de l'OU **LabUsers** selon les critères de sécurité définis.     


## Q.1.3.1 Créer une GPO "Drive-Mount" qui monte les lecteurs E: et F: sur les clients     


1. **Ouvrir le Gestionnaire de Stratégie de Groupe (Group Policy Management) :**      


- Ouvrir le **Server Manager** et allez dans **Tools > Group Policy Management**.     


<img width="624" alt="1_newgpo" src="https://github.com/user-attachments/assets/52b31c35-91bd-48a5-8193-c1df17aedcca">    



2. **Créer une nouvelle GPO :**     

- Dans l'arborescence du domaine **TSSR.LAN**, faites un clic droit sur l'OU **LabUsers** et sélectionnez **Create a GPO in this domain, and Link it here...**.      


<img width="194" alt="2_GPOname" src="https://github.com/user-attachments/assets/4cba76cc-a6d9-4cf8-a09c-fa835b72ac4b">    




- Nommer la nouvelle GPO **Drive-Mount** et cliquez sur **OK**.    


3. **Configurer la GPO pour monter les lecteurs réseau :**     


- Faire un clic droit sur la nouvelle GPO **Drive-Mount** et sélectionnez **Edit**.     


<img width="624" alt="3_editGPO" src="https://github.com/user-attachments/assets/9cb91f55-0dd3-43b9-b9bb-dd9671d69b53">    


- Dans l'éditeur de stratégie de groupe, naviguez vers **User Configuration > Preferences > Windows Settings > Drive Maps**.     


<img width="402" alt="4_newmappeddrive" src="https://github.com/user-attachments/assets/74ef1ad0-ff30-4d47-a51f-73db1f616cd3">     



4. **Ajouter un nouveau lecteur mappé pour le lecteur E: :**      


- Faites un clic droit sur **Drive Maps** et sélectionner **New > Mapped Drive**.     


<img width="198" alt="5_confdrive" src="https://github.com/user-attachments/assets/d64275ee-079d-4e79-8967-bd4ef86436b4">    



- Dans la fenêtre **New Drive Properties**, configurer les paramètres suivants :     

- **Action** : Update    

- **Location** : E:\    

- **Reconnect** : Coché    

- **Drive Letter** : E:    

- Cliquez sur **OK**.     


<img width="198" alt="6_confnew" src="https://github.com/user-attachments/assets/db980a1d-57e6-43c5-8116-a9803b993a76">     


   

5. **Ajouter un nouveau lecteur mappé pour le lecteur F: :**     

- Répétez les mêmes étapes que pour le lecteur E:, mais configurez **Location** pour F:\ et **Drive Letter** pour F:.    


<img width="200" alt="7_rebF" src="https://github.com/user-attachments/assets/669b3ccb-5e30-450b-aed2-a9b5c6fcc011">    


6. **Vérifier la configuration :**    

- Vérifiez que les lecteurs E: et F: apparaissent dans la liste sous **Drive Maps**.    


7. **Appliquer et lier la GPO :**    

- Retournez dans le **Group Policy Management**, faites un clic droit sur l'OU **LabUsers**, puis sélectionnez **Link an Existing GPO...** et choisissez **Drive-Mount**.     

<img width="391" alt="9_proof" src="https://github.com/user-attachments/assets/1e3c9202-4dec-4e18-9d7f-10eb4e1384ee">     


   
- Assurez-vous que la GPO est activée et appliquée correctement.    


8. **Vérifier l'application de la stratégie :**    

- Exécutez `gpupdate /force` sur un client du domaine pour appliquer les changements.    

- Utilisez `gpresult /r` pour vérifier que la GPO "Drive-Mount" est appliquée aux utilisateurs.    

