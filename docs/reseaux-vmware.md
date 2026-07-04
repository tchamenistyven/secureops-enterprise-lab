# Réseaux virtuels VMware

## Objectif

Les réseaux virtuels VMware séparent les différentes zones de l'infrastructure avant même la création des machines virtuelles. Cette segmentation permet de préparer un laboratoire plus proche d'une architecture d'entreprise : chaque zone a un rôle précis, et les communications entre zones seront contrôlées plus tard par pfSense.

Ce document décrit la configuration VMware réellement réalisée pour SecureOps Enterprise Lab. Il ne décrit pas un test de bout en bout, car pfSense et les autres machines virtuelles ne sont pas encore créés.

## Configuration finale

| Réseau | Type | Sous-réseau | Adaptateur hôte | DHCP VMware | Utilisation |
| --- | --- | --- | --- | --- | --- |
| `VMnet0` | Bridged | réseau physique automatique | Non applicable | Non applicable | Non utilisé par SecureOps |
| `VMnet1` | Host-only | `192.168.79.0/24` | Activé | Activé | Réseau VMware par défaut, non utilisé |
| `VMnet8` | NAT | `192.168.200.0/24` | Activé | Activé | WAN de pfSense et accès Internet |
| `VMnet2` | Host-only | `192.168.10.0/24` | Activé | Désactivé | Administration |
| `VMnet3` | Host-only isolé, affiché Custom | `192.168.20.0/24` | Désactivé | Désactivé | Serveurs |
| `VMnet4` | Host-only isolé, affiché Custom | `192.168.30.0/24` | Désactivé | Désactivé | Utilisateurs |
| `VMnet5` | Host-only isolé, affiché Custom | `192.168.40.0/24` | Désactivé | Désactivé | DMZ |
| `VMnet6` | Host-only isolé, affiché Custom | `192.168.50.0/24` | Désactivé | Désactivé | Sauvegarde |

## VMnet8 — WAN

`VMnet8` utilise le NAT VMware et fournit l'accès à Internet aux machines qui seront connectées côté WAN. Son sous-réseau est `192.168.200.0/24` et le service DHCP VMware reste activé.

L'interface WAN de `fw-01` recevra automatiquement une adresse depuis ce réseau. `VMnet8` était initialement en `192.168.30.0/24`; il a été déplacé vers `192.168.200.0/24` pour éviter un conflit avec le futur réseau Utilisateurs.

## VMnet2 — Administration

`VMnet2` correspond au réseau Administration en `192.168.10.0/24`. Le DHCP VMware est désactivé et l'adaptateur hôte est activé.

L'adaptateur Windows `VMware Network Adapter VMnet2` a été configuré comme suit :

| Paramètre | Valeur |
| --- | --- |
| Adresse IPv4 | `192.168.10.254` |
| Masque | `255.255.255.0` |
| Préfixe | `/24` |
| Passerelle | aucune |
| Serveur DNS | aucun |

`fw-01` utilisera `192.168.10.1/24` sur cette zone. Ce réseau permettra au PC hôte Windows d'administrer pfSense et les équipements autorisés du réseau Administration.

## VMnet3 — Serveurs

`VMnet3` correspond au réseau Serveurs en `192.168.20.0/24`. L'adaptateur hôte est désactivé et le DHCP VMware est désactivé.

Les serveurs utiliseront des adresses statiques. Le trafic vers les autres zones devra passer par pfSense.

## VMnet4 — Utilisateurs

`VMnet4` correspond au réseau Utilisateurs en `192.168.30.0/24`. L'adaptateur hôte est désactivé et le DHCP VMware est désactivé.

pfSense fournira temporairement le DHCP pour cette zone. Windows Server DHCP sera ajouté plus tard. La plage prévue pour les clients est `192.168.30.100` à `192.168.30.199`.

## VMnet5 — DMZ

`VMnet5` correspond à la DMZ en `192.168.40.0/24`. L'adaptateur hôte est désactivé et le DHCP VMware est désactivé.

`lb-01` utilisera `192.168.40.10/24` et pfSense utilisera `192.168.40.1/24`. Cette zone hébergera le point d'entrée HAProxy.

## VMnet6 — Sauvegarde

`VMnet6` correspond au réseau Sauvegarde en `192.168.50.0/24`. L'adaptateur hôte est désactivé et le DHCP VMware est désactivé.

`backup-01` utilisera `192.168.50.10/24` et pfSense utilisera `192.168.50.1/24`. Les flux de sauvegarde seront contrôlés par le pare-feu.

## Réseaux VMware non utilisés

`VMnet0` reste disponible en mode Bridged, mais ne sera pas utilisé par SecureOps Enterprise Lab.

`VMnet1` reste disponible en Host-only avec le réseau VMware par défaut `192.168.79.0/24`, mais ne sera pas utilisé par SecureOps Enterprise Lab.

Ces réseaux ne doivent pas être supprimés, car ils appartiennent à la configuration VMware par défaut.

## Principes de sécurité

- Les zones internes sont séparées.
- Seul `VMnet2` possède un adaptateur hôte pour permettre l'administration depuis Windows.
- Les autres réseaux internes ne sont pas directement accessibles par Windows.
- Aucun DHCP VMware n'est actif sur `VMnet2` à `VMnet6`.
- pfSense assurera le routage entre les zones.
- Tout trafic non autorisé sera bloqué par défaut.

## État actuel

- Les réseaux VMware sont créés.
- pfSense n'est pas encore installé.
- Aucun routage inter-zones n'est encore disponible.
- Les machines virtuelles applicatives ne sont pas encore créées.
- Les réseaux ne sont donc pas encore testables de bout en bout.

## Futures interfaces pfSense

| Interface pfSense | Réseau VMware | Adresse prévue |
| --- | --- | --- |
| WAN | `VMnet8` | DHCP VMware sur `192.168.200.0/24` |
| ADMIN | `VMnet2` | `192.168.10.1/24` |
| SERVERS | `VMnet3` | `192.168.20.1/24` |
| USERS | `VMnet4` | `192.168.30.1/24` |
| DMZ | `VMnet5` | `192.168.40.1/24` |
| BACKUP | `VMnet6` | `192.168.50.1/24` |
