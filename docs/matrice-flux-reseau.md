# Matrice initiale des flux réseau

Cette matrice décrit les flux prévus pour la conception initiale. Elle servira de base aux règles pfSense, mais ne remplace pas les tests réels qui seront faits lors de la configuration.

Principe de sécurité :

```text
Tout ce qui n'est pas explicitement autorisé doit être bloqué par défaut.
```

| Source | Destination | Ports ou services | Action prévue | Justification |
| --- | --- | --- | --- | --- |
| Administration | pfSense | HTTPS | Autoriser | Administration du pare-feu |
| Administration | Serveurs Ubuntu | SSH 22 | Autoriser | Administration Linux |
| Administration | Windows Server | RDP 3389 | Autoriser | Administration Windows |
| Administration | Grafana | HTTP/HTTPS | Autoriser | Consultation des tableaux de bord |
| Utilisateurs | `dc-01` | DNS 53 TCP/UDP | Autoriser | Résolution DNS |
| Utilisateurs | `dc-01` | Kerberos 88 | Autoriser | Authentification du domaine |
| Utilisateurs | `dc-01` | LDAP/LDAPS 389/636 | Autoriser | Services Active Directory |
| Utilisateurs | `dc-01` | SMB 445 | Autoriser selon besoin | GPO et partages |
| Utilisateurs | `lb-01` | HTTP/HTTPS 80/443 | Autoriser | Accès à l'application |
| Utilisateurs | Administration | Tous | Bloquer | Séparation des responsabilités |
| Utilisateurs | Serveurs applicatifs directs | Tous | Bloquer | Passage obligatoire par HAProxy |
| `lb-01` | `app-01` et `app-02` | HTTP ou port applicatif | Autoriser | Reverse proxy et health checks |
| DMZ | `dc-01` | Tous | Bloquer par défaut | Réduction du risque |
| `monitor-01` | Serveurs Ubuntu | Node Exporter 9100 | Autoriser | Collecte des métriques Linux |
| `monitor-01` | Windows Server | Windows Exporter 9182 | Autoriser | Collecte des métriques Windows |
| `monitor-01` | `lb-01` et applications | HTTP/HTTPS | Autoriser | Sondes Blackbox |
| `backup-01` | Serveurs Ubuntu | SSH 22 | Autoriser selon besoin | Sauvegarde par extraction |
| `backup-01` | Windows Server | SMB ou protocole choisi | Autoriser selon besoin | Sauvegarde Windows |
| Réseaux internes | Internet | HTTP/HTTPS, DNS, NTP | Autoriser selon zone | Mises à jour |
| Internet | Réseaux internes | Tous | Bloquer par défaut | Protection du laboratoire |

## Notes

Les ports Active Directory seront détaillés lors de la configuration réelle de `dc-01`, car certains services Windows nécessitent des flux complémentaires selon les rôles activés, les GPO, l'administration distante et les besoins de réplication.

La DMZ doit rester isolée des services internes sauf exception explicitement justifiée. Les utilisateurs ne doivent pas atteindre directement les serveurs applicatifs : le chemin applicatif normal passe par `lb-01`.
