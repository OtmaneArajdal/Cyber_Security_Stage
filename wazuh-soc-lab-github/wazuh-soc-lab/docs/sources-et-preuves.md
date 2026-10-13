# Sources et preuves du projet

La présentation publique est basée sur le rapport de stage « Supervision d’un SOC avec Wazuh », rédigé par Otmane Arajdal pour son stage technique de quatrième année, et sur les captures originales du laboratoire. Le rapport complet de 53 pages est conservé hors du dépôt car une capture y expose un accès administrateur. La synthèse publique ne reprend ni ce secret, ni l’adresse de l’organisme, ni les coordonnées de tiers.

Les numéros ci-dessous désignent les positions des pages du PDF source, et non les folios imprimés.

| Élément documenté | Rapport source | Preuve conservée |
|---|---|---|
| Contexte du stage de 4e année, juillet-septembre 2026 | Page 1 | Synthèse publique |
| Server, Indexer et Dashboard sur le nœud Ubuntu | Pages 30-31 et 39 | Architecture logique publique |
| Agent Kali installé et actif | Pages 41-45 | [Agent actif](../assets/agent-actif.jpeg) |
| Tentatives locales de changement d’identité | Pages 47-49 | [Test local](../assets/test-authentification-locale.jpeg) et [alertes PAM](../assets/alertes-pam.jpeg) |
| Essai SSH local et événements de règle 5710 | Pages 48-50 | [Essai Hydra](../assets/test-ssh-local.jpeg) et [alertes SSH](../assets/alertes-ssh.jpeg) |
| Compteur agrégé d’échecs d’authentification | Pages 50-51 | [Vue sur 24 heures](../assets/vue-authentifications.jpeg) |
| Difficultés d’installation et perspectives | Pages 51-52 | Documentation de déploiement et bilan |

Les captures conservées sont des copies inchangées des images sélectionnées. Le schéma d’architecture est une représentation logique préparée pour ce dépôt, pas une capture du réseau original. Les textes distinguent les résultats visibles des recommandations et des fonctions non validées.

La règle 5710 et les niveaux sont interprétés avec les descriptions visibles et la documentation officielle. Les règles de la branche actuelle peuvent évoluer : la version visible pendant le stage était Wazuh 4.7.5.
