# Vérification des outils de travail

## Étape 0.2.1 — Inventaire initial

Date de vérification : 04/07/2026 13:09:05

Chemin du projet :

```text
C:\Users\STYVEN\Documents\secureops-enterprise-lab
```

## Logiciels

| Outil | État | Informations |
| --- | --- | --- |
| VMware Workstation Pro | INSTALLÉ | C:\Program Files (x86)\VMware\VMware Workstation\vmware.exe — version 17.6.1 build-24319023 |
| Git | NON DÉTECTÉ | Aucun exécutable trouvé dans le PATH, les emplacements courants ou les clés de désinstallation Windows |
| Visual Studio Code | NON DÉTECTÉ | Aucun exécutable trouvé dans le PATH, les emplacements courants ou les clés de désinstallation Windows |
| diagrams.net Desktop | NON DÉTECTÉ | Facultatif : la version web peut être utilisée |
| WinGet | DISPONIBLE | v1.29.280 — C:\Users\STYVEN\AppData\Local\Microsoft\WindowsApps\winget.exe |

## Ordinateur hôte

| Information | Résultat |
| --- | --- |
| Système Windows | Microsoft Windows 10 Professionnel |
| Version Windows | 10.0.19045 |
| Numéro de build | 19045 |
| Fabricant et modèle | Micro-Star International Co., Ltd. Cyborg 15 A12VF |
| Mémoire RAM | 15.71 Go |
| Processeur | 12th Gen Intel(R) Core(TM) i7-12650H |
| Virtualisation activée dans le BIOS ou l'UEFI | False |
| SLAT disponible | False |
| Extensions VM Monitor disponibles | False |

## Vérification complémentaire systeminfo

```text
                                            [08]: Hyper-V Virtual Ethernet Adapter
Configuration requise pour Hyper-V:         Un hyperviseur a été détecté. Les fonctionnalités nécessaires à Hyper-V ne seront pas affichées.
```

## Outils prêts

- VMware Workstation Pro : INSTALLÉ — C:\Program Files (x86)\VMware\VMware Workstation\vmware.exe — version 17.6.1 build-24319023
- WinGet : DISPONIBLE — v1.29.280 — C:\Users\STYVEN\AppData\Local\Microsoft\WindowsApps\winget.exe

## Outils manquants

- Git : NON DÉTECTÉ — Aucun exécutable trouvé dans le PATH, les emplacements courants ou les clés de désinstallation Windows
- Visual Studio Code : NON DÉTECTÉ — Aucun exécutable trouvé dans le PATH, les emplacements courants ou les clés de désinstallation Windows
- diagrams.net Desktop : NON DÉTECTÉ — Facultatif : la version web peut être utilisée

## Points à vérifier

- Virtualisation matérielle : À CONFIRMER — Get-CimInstance retourne False, mais systeminfo indique qu'un hyperviseur est détecté et masque les prérequis Hyper-V.

## Erreurs rencontrées

Aucune erreur rencontrée.

## Méthodes alternatives utilisées

- Git : recherche dans les emplacements d'installation courants et les clés de désinstallation Windows, car la commande n'est pas dans le PATH.
- Visual Studio Code : recherche dans les clés de désinstallation Windows, en plus du PATH et des emplacements courants.
- diagrams.net Desktop : recherche dans les clés de désinstallation Windows, en plus des emplacements courants.

## Interprétation

* True pour la virtualisation indique qu'elle est activée.
* False peut indiquer qu'elle est désactivée dans le BIOS ou l'UEFI, ou que l'état est masqué par un hyperviseur déjà actif.
* Non déterminé signifie qu'une vérification manuelle ou complémentaire sera nécessaire.
* diagrams.net Desktop est facultatif, car la version web peut être utilisée.
* Aucun logiciel manquant ne doit être installé avant validation.

## Statut

Étape 0.2.1 exécutée. En attente de vérification et de validation.

