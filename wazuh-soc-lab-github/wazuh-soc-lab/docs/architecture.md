# Architecture du laboratoire

![Architecture logique](../assets/architecture.png)

Le périmètre pratique documenté comprend un nœud Ubuntu 22.04 LTS qui héberge Wazuh Server, Indexer et Dashboard, ainsi qu’un agent sur Kali Linux. L’assistant d’installation et le paquet de l’agent montrent la version 4.7.5. Le rapport décrit un réseau virtuel NAT.

L’agent transmet les événements au Server. Le Server applique les décodeurs et les règles. Les alertes sont ensuite indexées et consultées dans le Dashboard. Le schéma est volontairement fonctionnel : il résume ce chemin et ne constitue pas une copie des paramètres du poste.

## Flux de référence

| Flux | Port de référence | Utilité |
|---|---|---|
| Agent vers Server | 1514/TCP | Transmission des événements |
| Agent vers service d’enrôlement | 1515/TCP | Enregistrement de l’agent |
| Composants centraux vers Indexer | 9200/TCP | Indexation et interrogation |
| Dashboard vers API Server | 55000/TCP | Administration et état des agents |
| Analyste vers Dashboard | 443/TCP par défaut | Interface web |

Ces ports sont les références de l’architecture Wazuh. Le projet ne contient pas une capture de chaque règle de pare-feu. Les images du Dashboard montrent une adresse locale ; elles ne suffisent pas à reconstituer l’ensemble des accès ou à attester le chiffrement de chaque chemin.

## Périmètre de validation

Un agent Kali actif est visible. Aucun enrôlement Windows ni capteur réseau indépendant n’est démontré. Un schéma plus large du rapport représente un Indexer séparé, un client supplémentaire et un réseau Radmin ; les captures de déploiement documentent surtout l’installation all-in-one et l’agent Kali. Le schéma public ci-dessus retient ce périmètre démontré.

Pour une nouvelle réalisation, il faudrait figer les versions, conserver la configuration exacte du réseau et des sources de journaux, puis vérifier les flux autorisés. NAT ne remplace pas un contrôle des règles d’accès.

Référence : [architecture officielle Wazuh](https://documentation.wazuh.com/current/getting-started/architecture.html).
