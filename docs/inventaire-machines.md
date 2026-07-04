# Inventaire des machines virtuelles

Ce document décrit les machines virtuelles prévues pour SecureOps Enterprise Lab. Les valeurs de ressources sont des valeurs de conception et pourront être adaptées lors du déploiement selon les performances de l'ordinateur hôte.

Toutes les machines Linux prévues utiliseront Ubuntu Desktop avec interface graphique.

| VM | Système | Rôle | Services prévus | Ressources prévues | Zone réseau | Adresse IP | Cartes réseau | Dépendances principales | Statut actuel |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| `fw-01` | pfSense | Pare-feu, routage inter-zones, NAT et filtrage | pfSense, règles firewall, NAT, routage, éventuellement DNS forwarding | 2 vCPU, 4 Go RAM, 20 Go disque | WAN, Administration, Serveurs, Utilisateurs, DMZ, Sauvegarde | WAN DHCP sur `VMnet8`, puis `192.168.10.1/24`, `192.168.20.1/24`, `192.168.30.1/24`, `192.168.40.1/24`, `192.168.50.1/24` | 6 | VMware Workstation et réseaux VMnet | Planifiée — non créée |
| `dc-01` | Windows Server | Contrôleur de domaine et services d'identité | Active Directory DS, DNS, DHCP, GPO, Windows LAPS | 4 vCPU, 8 Go RAM, 100 Go disque | Serveurs | `192.168.20.10/24` | 1 | `fw-01`, réseau Serveurs | Planifiée — non créée |
| `client-01` | Windows 11 | Poste utilisateur membre du domaine | Client Windows, jonction domaine, GPO, accès applicatif | 4 vCPU, 8 Go RAM, 80 Go disque | Utilisateurs | DHCP, plage prévue `192.168.30.100-199` | 1 | `fw-01`, `dc-01` | Planifiée — non créée |
| `ops-01` | Ubuntu Desktop | Poste d'administration | Git, GitHub CLI, Ansible, scripts d'administration, outils réseau | 2 vCPU, 4 Go RAM, 60 Go disque | Administration | `192.168.10.10/24` | 1 | `fw-01`, accès aux zones administrées | Planifiée — non créée |
| `app-01` | Ubuntu Desktop | Serveur applicatif principal | Python Flask, Nginx, Docker selon évolution | 2 vCPU, 4 Go RAM, 60 Go disque | Serveurs | `192.168.20.31/24` | 1 | `fw-01`, `lb-01`, `dc-01` pour DNS | Planifiée — non créée |
| `app-02` | Ubuntu Desktop | Serveur applicatif secondaire | Python Flask, Nginx, Docker selon évolution | 2 vCPU, 4 Go RAM, 60 Go disque | Serveurs | `192.168.20.32/24` | 1 | `fw-01`, `lb-01`, `dc-01` pour DNS | Planifiée — non créée |
| `lb-01` | Ubuntu Desktop | Point d'entrée applicatif | HAProxy, health checks, reverse proxy | 2 vCPU, 4 Go RAM, 40 Go disque | DMZ | `192.168.40.10/24` | 1 | `fw-01`, `app-01`, `app-02` | Planifiée — non créée |
| `monitor-01` | Ubuntu Desktop | Supervision et observabilité | Prometheus, Grafana, Alertmanager, Blackbox Exporter, Loki | 4 vCPU, 8 Go RAM, 100 Go disque | Serveurs | `192.168.20.20/24` | 1 | Connectivité vers les machines supervisées | Planifiée — non créée |
| `backup-01` | Ubuntu Desktop | Sauvegarde et restauration | Restic ou outil équivalent, stockage d'archives, tests de restauration | 2 vCPU, 4 Go RAM, 120 Go disque | Sauvegarde | `192.168.50.10/24` | 1 | Services à sauvegarder, `fw-01` | Planifiée — non créée |

## Synthèse des rôles

- `fw-01` sépare et protège les zones.
- `dc-01` fournit l'identité, le DNS, le DHCP et les politiques Windows.
- `client-01` représente le poste utilisateur.
- `ops-01` sert à administrer le laboratoire.
- `app-01` et `app-02` hébergeront l'application.
- `lb-01` publiera l'application via HAProxy.
- `monitor-01` supervisera l'environnement.
- `backup-01` portera les sauvegardes et les restaurations.
