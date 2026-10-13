# Démarche de déploiement

Ce document décrit les étapes suivies pendant le stage. Il ne constitue pas un script d’installation automatique ni un export complet des configurations originales.

## Préparer les machines

J’ai préparé un serveur Ubuntu et une machine Kali dans le laboratoire virtuel, puis vérifié l’adressage et la connectivité. Le rapport décrit un réseau NAT. La taille CPU/RAM présentée dans le rapport correspond à des recommandations de dimensionnement ; les captures ne permettent pas d’attester toutes les allocations réellement utilisées.

## Installer les composants centraux

L’assistant officiel `wazuh-install.sh` a été lancé en mode all-in-one. Les traces d’installation montrent Wazuh 4.7.5. Le nœud central regroupe le Server, l’Indexer et le Dashboard. Pour reproduire la démarche aujourd’hui, utiliser la [documentation officielle à jour](https://documentation.wazuh.com/current/quickstart.html) et une version maintenue, plutôt qu’exécuter sans vérification les commandes historiques.

Les identifiants générés sont des données privées de l’environnement. La capture les affichant est exclue de ce dépôt. L’accès au Dashboard a permis de préparer l’enrôlement de l’agent.

## Installer et enrôler l’agent

J’ai sélectionné un paquet Linux DEB dans l’assistant du Dashboard, indiqué le Manager et nommé l’agent Kali. Le paquet agent 4.7.5-1 a été installé sur le poste, puis le service `wazuh-agent` a été activé. Une revue du fichier `ossec.conf` et le statut actif du Dashboard ont confirmé l’intégration.

Contrôles de service possibles dans un laboratoire Linux :

```bash
sudo systemctl status wazuh-agent
sudo systemctl status ssh
```

Ces commandes vérifient l’état de services ; elles ne remplacent pas le contrôle des journaux effectivement collectés.

## Préparer les observations

Le service SSH a été activé sur Kali. Les essais locaux ont utilisé un compte dédié, puis des échecs d’authentification volontaires. Les sources observées comprennent les messages PAM et SSH. Les captures fournies ne constituent pas un export réutilisable et complet de `ossec.conf` : aucune configuration copiée partiellement n’est distribuée comme modèle validé.

## Difficultés rencontrées

Le rapport décrit des blocages liés à la gestion des paquets et des fichiers résiduels de l’installation. La leçon retenue est de commencer par identifier le processus ou le service responsable et de relire les journaux. Les opérations de purge réalisées pendant le dépannage ne sont pas proposées comme procédure générique : elles peuvent supprimer des données et dépendent de l’état précis de la machine.

## Ce qui manque pour une reproduction identique

Le dépôt ne contient ni export des VM, ni configurations finales complètes, ni journal brut horodaté de chaque essai. Il permet d’étudier la démarche et les résultats. Une reproduction demanderait de préparer ces éléments et de conserver des preuves séparées pour chaque scénario.
