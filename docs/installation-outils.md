# Installation des outils de travail

## Étape 0.2.2 — Git et Visual Studio Code

Date de l'intervention : 04/07/2026 13:26:17

## Dossier du projet

`C:\Users\STYVEN\Documents\secureops-enterprise-lab`

## WinGet

- État : disponible
- Version : `v1.29.280`
- Mise à jour des sources :

```text
Code sortie 0
Mise à jour en cours de toutes les sources... Merci de patienter.
Mise à jour de la source : msstore...
Terminé
Mise à jour de la source : winget...
Terminé
Mise à jour de la source : winget-font...
Terminé
```

## Git

- État avant l'étape : non détecté
- Action : installation avec WinGet
- Identifiant WinGet : `Git.Git`
- Résultat : installation exécutée avec succès par WinGet, puis vérifiée par commande et par WinGet.
- Version installée : `git version 2.55.0.windows.1`
- Emplacement : `C:\Program Files\Git\cmd\git.exe`
- Commande `git` disponible : Oui

## Visual Studio Code

- État avant l'étape : non détecté
- Action : installation avec WinGet
- Identifiant WinGet : `Microsoft.VisualStudioCode`
- Résultat : installation exécutée avec succès par WinGet, puis vérifiée par commande, par fichier exécutable et par WinGet.
- Version exécutable déterminée : `1.126.0`
- Version `Code.exe` : `1.126.0`
- Version affichée par WinGet : voir la section de vérification finale WinGet.
- Emplacement de la commande : `C:\Users\STYVEN\AppData\Local\Programs\Microsoft VS Code\bin\code.cmd`
- Emplacement de l'application : `C:\Users\STYVEN\AppData\Local\Programs\Microsoft VS Code\Code.exe`
- Commande `code` disponible : Oui
- Script `code.cmd` détecté : `C:\Users\STYVEN\AppData\Local\Programs\Microsoft VS Code\bin\code.cmd`
- Ouverture du dossier du projet : Réussie avec la commande code

## Vérification finale WinGet

### Git

```text
Nom ID      Version
-------------------
Git Git.Git 2.55.0
```

### Visual Studio Code

```text
Nom                                 ID                         Version
----------------------------------------------------------------------
Microsoft Visual Studio Code (User) Microsoft.VisualStudioCode 1.127.0
```

## Outils non installés volontairement

- diagrams.net Desktop : non installé, car facultatif.

## Problèmes rencontrés

- La vérification initiale `winget list --id ... --exact` sans source explicite a demandé l'acceptation interactive des contrats de la source `msstore` et a retourné `0x8a150042 : Erreur lors de la lecture de l'entrée dans l'invite`.
- Visual Studio Code indique `1.126.0` via `code --version` et `Code.exe`, tandis que `winget list` affiche `1.127.0` pour le paquet installé. La version exécutable retenue est donc `1.126.0`, avec la sortie WinGet conservée en vérification complémentaire.

## Corrections appliquées

- PATH de la session PowerShell actualisé depuis les variables Machine et User.
- Vérification WinGet relancée avec `--source winget --accept-source-agreements`, ce qui a confirmé Git et Visual Studio Code sans interaction avec la source `msstore`.
- Version de Visual Studio Code revérifiée avec `code --version` et `Code.exe` après l'écart constaté avec `winget list`.

## Statut

En attente de validation de l'étape 0.2.2.
