# Analyse des résultats et limites

Ce projet a été réalisé pendant mon stage technique de quatrième année, en 2026. J’y ai déployé une base de supervision SOC avec Wazuh, puis examiné des événements d’authentification Linux. Mon analyse porte sur ce que les captures permettent réellement d’observer. Je distingue la mise en service de l’environnement, la visibilité des événements et la validation d’une détection d’attaque : ces trois résultats ne sont pas équivalents.

## Ce que j’ai pu vérifier

L’enregistrement de l’agent et son état actif dans le Dashboard constituent la première validation du déploiement. Les événements d’authentification consultables apportent ensuite une preuve plus concrète de la collecte et de l’analyse. J’ai pu retrouver des refus PAM, des vérifications de mot de passe échouées et des tentatives SSH avec un utilisateur inexistant.

| Capture conservée | Résultat observable | Limite d’interprétation |
|---|---|---|
| [Agent actif](../assets/agent-actif.jpeg) | Agent enregistré et connecté | Ne valide pas tous les modules |
| [Alertes PAM](../assets/alertes-pam.jpeg) | Règles 5301, 5503 et 5557, niveau 5 | Aucun accès privilégié réussi démontré |
| [Alertes SSH](../assets/alertes-ssh.jpeg) | Règle 5710 : utilisateur inexistant | Aucune corrélation 5712 validée |
| [Vue des authentifications](../assets/vue-authentifications.jpeg) | 83 événements, dont 52 échecs et un succès, sur 24 heures | Compteurs non attribuables à un seul essai |

La vue agrégée facilite le repérage d’une activité inhabituelle, mais elle nécessite une investigation. Les 52 échecs ne correspondent pas nécessairement à 52 actions distinctes : plusieurs journaux peuvent être associés à la même tentative. Le succès affiché ne prouve pas non plus que le test Hydra a obtenu un accès.

## Interpréter les règles avec précision

Les identifiants de règles aident à comprendre le traitement effectué. Les événements 5301, 5503 et 5557 renseignent des échecs d’authentification à différents niveaux du système. Leur niveau 5 ne justifie pas le terme « critique ». La [classification officielle de Wazuh](https://documentation.wazuh.com/current/user-manual/ruleset/rules/rules-classification.html) décrit notamment ce niveau comme celui d’erreurs liées à l’utilisateur.

Pour SSH, la règle 5710 signale une tentative avec un utilisateur inexistant. Je ne la présente pas comme une preuve de corrélation d’une attaque répétée. Les [règles SSH](https://github.com/wazuh/wazuh/blob/master/ruleset/rules/0095-sshd_rules.xml) distinguent cette observation d’une règle de corrélation comme 5712. Les [règles PAM](https://github.com/wazuh/wazuh/blob/master/ruleset/rules/0085-pam_rules.xml) donnent également le contexte des événements observés.

L’essai Hydra documenté n’a trouvé aucun mot de passe valide. Les horaires des captures Hydra et Dashboard diffèrent, sans trace complète permettant de relier les deux. Je peux donc démontrer la présence d’un essai automatisé et d’événements SSH, mais pas attribuer chaque alerte à cet essai ni affirmer une compromission.

## Limites du périmètre et suite envisagée

Le laboratoire repose sur un serveur central et un agent Kali. Il ne représente pas un SOC de production, une supervision permanente ni un parc hétérogène. Je n’ai pas mesuré le taux de détection, les faux positifs ou le délai entre l’action et l’alerte. Les fonctions d’intégrité des fichiers, d’analyse des vulnérabilités et de réponse automatique ne disposent pas ici d’une validation pratique documentée.

La suite consisterait à isoler chaque scénario dans une fenêtre temporelle, vérifier les événements attendus et conserver des exports anonymisés. Je souhaiterais aussi tester la corrélation SSH, ajouter un poste Windows et étudier une procédure de réponse. Ces éléments sont des perspectives. Le principal acquis de ce stage reste ma capacité à déployer la collecte, retrouver un événement et formuler une conclusion proportionnée aux preuves disponibles.
