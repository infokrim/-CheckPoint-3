# Exercice 1 : Manipulations pratiques sur VM Windows

## Partie 1 : Gestion des utilisateurs

**Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs de société que Kelly Rhameur.**

1. **Copier l'utilisateur existant :**
   - Cliquez avec le bouton droit sur **Kelly Rhameur** dans la console **Active Directory Users and Computers** et sélectionnez **Copy**.
     

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
   <img width="410" alt="Q111f-CUchangeproperties" src="https://github.com/user-attachments/assets/0d7e4462-249c-41b7-a506-ee6f23eb8f44">    

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





