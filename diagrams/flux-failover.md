# Flux de basculement automatique prévu

Ce document décrit une automatisation future. Elle n'existe pas encore et ne sera pas configurée pendant la phase de conception.

```mermaid
flowchart LR
    FAILURE[Panne app-01]
    BLACKBOX[Blackbox Exporter<br/>sonde HTTP]
    PROM[Prometheus<br/>règle d'alerte]
    ALERT[Alertmanager]
    WEBHOOK[Webhook Python]
    VMWARE[VMware Workstation<br/>démarrage app-02]
    HAPROXY[HAProxy<br/>contrôle backend]
    TRAFFIC[Trafic utilisateur<br/>redirigé vers app-02]

    FAILURE --> BLACKBOX
    BLACKBOX --> PROM
    PROM --> ALERT
    ALERT --> WEBHOOK
    WEBHOOK --> VMWARE
    VMWARE --> HAPROXY
    HAPROXY --> TRAFFIC
```

## Principe prévu

Lorsqu'une panne de `app-01` sera détectée, une sonde Blackbox Exporter alimentera Prometheus. Une règle d'alerte pourra déclencher Alertmanager, qui appellera un webhook Python. Ce webhook pourra ensuite lancer une action contrôlée sur VMware, vérifier l'état de `app-02`, puis laisser HAProxy rediriger le trafic vers le serveur disponible.

Cette logique sera validée beaucoup plus tard, après la création des machines, le déploiement de l'application, la mise en place de la supervision et les tests de panne.
