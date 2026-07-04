# Journal du projet SecureOps Enterprise Lab

## Phase 0 — Préparation

### Étape 0.1 — Création de la structure du projet

Actions réalisées :

- création du dossier principal `secureops-enterprise-lab` ;
- création des sous-dossiers du projet ;
- création du fichier README ;
- création du journal de projet ;
- choix d'Ubuntu Desktop avec interface graphique pour les machines Linux.

### Résultat

La structure initiale du projet a été créée.

### Problèmes rencontrés

Aucun problème pour le moment.

### Statut

En attente de validation de l'étape 0.1.

### Étape 0.2.1 — Inventaire des outils

Actions réalisées :

- vérification de VMware Workstation Pro ;
- vérification de Git ;
- vérification de Visual Studio Code ;
- vérification de diagrams.net Desktop ;
- vérification de WinGet ;
- vérification du système Windows ;
- vérification du processeur et de la virtualisation.

Résultats :

- VMware Workstation Pro : INSTALLÉ ;
- Git : NON DÉTECTÉ ;
- Visual Studio Code : NON DÉTECTÉ ;
- diagrams.net Desktop : NON DÉTECTÉ ;
- WinGet : DISPONIBLE ;
- virtualisation matérielle : False.

Rapport créé :

`docs/verification-outils.md`

Statut :

En attente de validation de l'étape 0.2.1.

### Étape 0.2.2 — Installation de Git et Visual Studio Code

Actions réalisées :

- vérification de WinGet ;
- installation ou vérification de Git ;
- installation ou vérification de Visual Studio Code ;
- vérification des commandes `git` et `code` ;
- ouverture du dossier du projet dans Visual Studio Code ;
- création du rapport d'installation.

Résultats :

- Git : installé et vérifié ;
- version de Git : git version 2.55.0.windows.1 ;
- Visual Studio Code : installé et vérifié ;
- version exécutable de Visual Studio Code : 1.126.0 ;
- version affichée par WinGet pour Visual Studio Code : 1.127.0 ;
- diagrams.net Desktop : non installé, car facultatif.

Rapport créé :

`docs/installation-outils.md`

Problèmes rencontrés :

La vérification `winget list` sans source explicite a demandé les contrats `msstore` en mode interactif ; la vérification a été relancée avec `--source winget --accept-source-agreements`. Un écart de version a aussi été constaté pour Visual Studio Code : `code --version` et `Code.exe` indiquent `1.126.0`, tandis que `winget list` affiche `1.127.0`.

Statut :

En attente de validation de l'étape 0.2.2.

### Étape 0.3.1 — Initialisation locale de Git

Date et heure : 04/07/2026 13:39:50

Actions réalisées :

- vérification de l'installation de Git ;
- initialisation du dépôt Git local ;
- définition de la branche principale `main` ;
- configuration locale de l'identité Git ;
- vérification et mise à jour du fichier `.gitignore` ;
- vérification de l'absence de fichiers sensibles ;
- ajout de fichiers `.gitkeep` dans les dossiers vides ;
- préparation du premier commit.

Configuration locale :

- nom Git : `Styven Tchameni` ;
- adresse e-mail Git : `noupoue.tchameni@hotmail.com` ;
- branche principale : `main`.

Aucun dépôt GitHub distant n'a encore été configuré.

Statut :

En attente de validation de l'étape 0.3.1.

### Étape 0.3.2 — Préparation sécurisée de GitHub

Date et heure : 04/07/2026 14:20:44

Actions réalisées :

- installation ou vérification de GitHub CLI ;
- authentification sécurisée avec le navigateur ;
- identification du compte GitHub ;
- activation ou vérification de la confidentialité des e-mails ;
- création de l'adresse GitHub `noreply` ;
- modification de l'adresse Git locale ;
- réécriture de l'historique local avant publication ;
- vérification des métadonnées des commits.

Résultats :

- compte GitHub : tchamenistyven ;
- GitHub CLI : gh version 2.96.0 (2026-07-02) ;
- adresse de commit : 239176593+tchamenistyven@users.noreply.github.com ;
- ancienne adresse présente dans `main` : Non ;
- remote configuré : Non ;
- push effectué : Non.

Rapport créé :

`docs/preparation-github.md`

Statut :

En attente de validation de l'étape 0.3.2.



### Étape 0.3.3 — Création et publication du dépôt GitHub

Date et heure : 04/07/2026 14:33:44

Actions réalisées :

- vérification finale du dépôt local ;
- vérification des auteurs et adresses des commits ;
- vérification de l'absence de données sensibles ;
- création du dépôt GitHub public ;
- configuration du remote `origin` ;
- publication de la branche `main` ;
- vérification de la synchronisation entre le dépôt local et GitHub ;
- ajout de la description et des sujets techniques ;
- vérification du dépôt dans le navigateur.

Résultats :

- dépôt : `tchamenistyven/secureops-enterprise-lab` ;
- visibilité : publique ;
- branche par défaut : `main` ;
- remote : `origin` ;
- URL : https://github.com/tchamenistyven/secureops-enterprise-lab ;
- fichiers sensibles publiés : Non ;
- push forcé utilisé : Non.

Rapport créé :

`docs/publication-github.md`

Statut :

En attente de validation de l'étape 0.3.3.


### Étape 0.5 — Conception de l'architecture initiale

Date et heure : 04/07/2026 15:10:46

Actions réalisées :

- définition des machines virtuelles ;
- définition des rôles et des services ;
- définition des ressources prévues ;
- définition des zones réseau ;
- création du plan d'adressage ;
- définition des cartes réseau ;
- création de la matrice initiale des flux ;
- création des schémas Mermaid ;
- mise à jour du README.

Documents créés ou complétés :

- `docs/architecture-overview.md` ;
- `docs/inventaire-machines.md` ;
- `docs/plan-adressage.md` ;
- `docs/matrice-flux-reseau.md` ;
- `diagrams/architecture-logique.md` ;
- `diagrams/architecture-reseau.md` ;
- `diagrams/flux-application.md` ;
- `diagrams/flux-failover.md`.

Résultat :

L'architecture cible est documentée. Aucune machine virtuelle ni aucun réseau VMware n'a encore été créé.

Statut :

En attente de validation de l'étape 0.5.


### Phase 1 — Création des réseaux VMware

Date et heure : 04/07/2026 22:19:12

#### Étape 1.1 — Analyse des réseaux existants

Résultats :

- VMnet0 détecté en mode Bridged ;
- VMnet1 détecté en mode Host-only ;
- VMnet8 détecté initialement en mode NAT sur 192.168.30.0/24 ;
- conflit identifié avec le futur réseau Utilisateurs.

#### Étape 1.2 — Création des réseaux internes

Actions réalisées :

- déplacement de VMnet8 vers 192.168.200.0/24 ;
- création de VMnet2 pour l'Administration ;
- création de VMnet3 pour les Serveurs ;
- création de VMnet4 pour les Utilisateurs ;
- création de VMnet5 pour la DMZ ;
- création de VMnet6 pour les Sauvegardes ;
- désactivation du DHCP VMware sur VMnet2 à VMnet6 ;
- désactivation des adaptateurs hôtes sur VMnet3 à VMnet6 ;
- configuration de l'adaptateur Windows VMnet2 en 192.168.10.254/24.

Résultat :

Les réseaux virtuels nécessaires à l'architecture ont été créés. Le routage sera assuré plus tard par pfSense.

Document créé :

`docs/reseaux-vmware.md`

Statut :

En attente de validation finale de la phase 1.
