# Architecture générale de SecureOps Enterprise Lab

## Objectif

SecureOps Enterprise Lab est un laboratoire virtualisé destiné à reproduire progressivement l'infrastructure d'une petite entreprise. L'objectif est d'apprendre, de documenter et de valider les composants essentiels d'un environnement d'administration systèmes et réseaux : segmentation, pare-feu, annuaire, postes clients, serveurs applicatifs, supervision, sauvegarde et automatisation.

Cette étape décrit l'architecture cible. Les machines virtuelles ne sont pas encore créées, l'application n'est pas encore développée et aucune configuration VMware, pfSense, Windows Server ou Ubuntu n'a encore été appliquée.

## Principes de conception

L'architecture repose sur quelques principes simples :

- séparer les usages dans des zones réseau distinctes ;
- faire passer les communications entre zones par le pare-feu `fw-01` ;
- placer l'annuaire, le DNS et le DHCP au centre des services internes ;
- exposer l'application aux utilisateurs via un point d'entrée contrôlé ;
- superviser les composants importants dès que les services seront déployés ;
- prévoir les sauvegardes et la restauration dès la conception.

Les ressources indiquées pour les machines virtuelles sont des valeurs de conception. Elles pourront être adaptées selon les performances réelles de l'ordinateur hôte.

## Zones réseau

Le laboratoire utilisera plusieurs réseaux VMware isolés :

- `VMnet8` pour le WAN VMware NAT ;
- `VMnet2` pour l'administration ;
- `VMnet3` pour les serveurs internes ;
- `VMnet4` pour les postes utilisateurs ;
- `VMnet5` pour la DMZ ;
- `VMnet6` pour la sauvegarde.

`fw-01`, basé sur pfSense, sera connecté à toutes les zones. Il assurera le routage, le NAT et le filtrage entre les réseaux. Les communications directes entre zones internes ne devront pas contourner ce pare-feu.

## Services principaux

`dc-01` portera les services Active Directory, DNS, DHCP, GPO et Windows LAPS. Il fournira le socle d'identité et de résolution de noms pour les machines internes.

Les machines Ubuntu Desktop auront plusieurs rôles :

- `ops-01` pour l'administration, Git, Ansible et les scripts ;
- `app-01` et `app-02` pour l'application web ;
- `lb-01` pour HAProxy ;
- `monitor-01` pour Prometheus, Grafana, Alertmanager, Blackbox Exporter et Loki ;
- `backup-01` pour les sauvegardes et les restaurations.

Toutes les machines Linux prévues utiliseront Ubuntu Desktop avec interface graphique, et non Ubuntu Server.

## Parcours d'un utilisateur

Un utilisateur travaillera depuis `client-01`, poste Windows 11 membre du domaine. Le poste utilisera `dc-01` pour le DNS et l'authentification Active Directory.

Pour accéder à l'application, l'utilisateur atteindra le nom applicatif interne, par exemple `app.secureops.test`. Le DNS renverra l'adresse de `lb-01`. Le trafic passera ensuite par `fw-01`, rejoindra la DMZ, puis HAProxy distribuera la requête vers `app-01` ou `app-02`.

L'application n'existe pas encore. Elle sera développée et déployée dans une étape ultérieure.

## Supervision et alertes

`monitor-01` centralisera l'observabilité. Prometheus collectera les métriques des serveurs, Grafana affichera les tableaux de bord, Alertmanager gérera les alertes et Blackbox Exporter testera la disponibilité des services exposés.

Loki est prévu pour la collecte et la consultation de journaux. Les détails de collecte seront définis lors de la configuration des machines et des services.

## Continuité de service

La conception prévoit deux serveurs applicatifs : `app-01` comme serveur principal et `app-02` comme serveur secondaire. HAProxy, installé sur `lb-01`, servira de point d'entrée applicatif et pourra rediriger le trafic selon l'état des serveurs.

Un mécanisme de basculement automatique est envisagé pour plus tard : détection d'une panne, alerte, webhook Python, action sur VMware et mise à jour du routage applicatif. Cette automatisation n'existe pas encore.

## Sauvegardes

`backup-01` sera dédié à la sauvegarde, à la restauration et à la conservation des archives. Les flux de sauvegarde seront limités au strict nécessaire et documentés avant leur activation.

Les outils précis de sauvegarde, les calendriers de rétention et les scénarios de restauration seront définis dans une étape ultérieure.

## Limites du laboratoire

Le laboratoire s'exécutera sur un seul ordinateur physique. Cette contrainte implique plusieurs limites :

- les pannes matérielles de l'hôte affecteront tout le laboratoire ;
- les performances dépendront de la mémoire, du processeur et du stockage disponibles ;
- la haute disponibilité restera simulée, car les machines partagent le même hôte ;
- les réseaux VMware reproduisent une segmentation logique, mais pas une infrastructure physique complète.

Ces limites sont acceptables pour un laboratoire d'apprentissage et de validation.

## Évolutions futures

Les prochaines étapes consisteront à préparer les images d'installation, créer les réseaux VMware, déployer les machines virtuelles, installer les systèmes, configurer pfSense, construire le domaine Active Directory, déployer l'application, puis ajouter l'automatisation, la supervision, les sauvegardes et les tests de restauration.
