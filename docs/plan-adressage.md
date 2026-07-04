# Plan d'adressage IP

## Conventions

Le laboratoire utilise des sous-réseaux IPv4 privés séparés par zone. Les communications entre zones doivent passer par `fw-01`, le pare-feu pfSense.

Les adresses indiquées sont des valeurs de conception. Elles seront appliquées progressivement lors de la création des réseaux VMware et des machines virtuelles.

## Réseaux VMware

| Réseau VMware | Zone | Sous-réseau | Passerelle |
| --- | --- | --- | --- |
| `VMnet8` | WAN VMware NAT | Attribué par VMware | Attribuée par VMware |
| `VMnet2` | Administration | `192.168.10.0/24` | `192.168.10.1` |
| `VMnet3` | Serveurs | `192.168.20.0/24` | `192.168.20.1` |
| `VMnet4` | Utilisateurs | `192.168.30.0/24` | `192.168.30.1` |
| `VMnet5` | DMZ | `192.168.40.0/24` | `192.168.40.1` |
| `VMnet6` | Sauvegarde | `192.168.50.0/24` | `192.168.50.1` |

## Interfaces pfSense

| Interface ou zone | Réseau VMware | Adresse prévue |
| --- | --- | --- |
| WAN | `VMnet8` | DHCP VMware NAT |
| Administration | `VMnet2` | `192.168.10.1/24` |
| Serveurs | `VMnet3` | `192.168.20.1/24` |
| Utilisateurs | `VMnet4` | `192.168.30.1/24` |
| DMZ | `VMnet5` | `192.168.40.1/24` |
| Sauvegarde | `VMnet6` | `192.168.50.1/24` |

## Adresses statiques

| Machine | Interface ou zone | Adresse |
| --- | --- | --- |
| `ops-01` | Administration | `192.168.10.10/24` |
| `dc-01` | Serveurs | `192.168.20.10/24` |
| `monitor-01` | Serveurs | `192.168.20.20/24` |
| `app-01` | Serveurs | `192.168.20.31/24` |
| `app-02` | Serveurs | `192.168.20.32/24` |
| `lb-01` | DMZ | `192.168.40.10/24` |
| `backup-01` | Sauvegarde | `192.168.50.10/24` |

## Plage DHCP utilisateurs

| Zone | Machine concernée | Plage prévue |
| --- | --- | --- |
| Utilisateurs | `client-01` et futurs postes clients | `192.168.30.100-192.168.30.199` |

Le service DHCP sera prévu sur `dc-01` pour la zone Utilisateurs. La configuration exacte sera validée lors de l'installation de Windows Server et de pfSense.

## Serveur DNS

Les machines internes utiliseront principalement :

```text
192.168.20.10
```

Cette adresse correspondra au serveur DNS de `dc-01`. Les redirecteurs DNS externes seront définis plus tard dans Windows Server ou pfSense.

## Adresses réservées

| Plage | Usage prévu |
| --- | --- |
| `.1` | Passerelle de la zone |
| `.2` à `.9` | Infrastructure future |
| `.10` à `.49` | Serveurs et services principaux |
| `.50` à `.99` | Réserve technique |
| `.100` à `.199` | DHCP utilisateurs lorsque la zone le permet |
| `.200` à `.254` | Réserve ou besoins ponctuels documentés |

## Règles de gestion

- Les passerelles utilisent l'adresse `.1`.
- Les serveurs utilisent des adresses statiques.
- Les clients utilisent DHCP lorsque cela est prévu.
- Les adresses `.2` à `.9` restent réservées à l'infrastructure future.
- Les adresses `.10` à `.49` sont réservées aux serveurs.
- Les adresses `.100` à `.199` peuvent être utilisées par DHCP.
- Une adresse IP ne doit jamais être attribuée à deux machines différentes.
- Tout changement d'adresse, de plage ou de passerelle devra être documenté dans ce fichier.
