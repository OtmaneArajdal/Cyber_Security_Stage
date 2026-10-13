# Scénarios de validation

Ce document présente les essais réalisés pendant mon stage technique de quatrième année, en 2026. Mon objectif était de vérifier que des événements d’authentification produits sur une machine Linux pouvaient être collectés, analysés et consultés dans Wazuh. Les captures documentent un laboratoire, avec une installation Wazuh 4.7.5 et un agent 4.7.5-1. Elles ne représentent pas une campagne d’attaque contre une infrastructure de production.

## Vérifier la chaîne de supervision

Avant les tests, j’ai contrôlé la présence de l’agent Kali dans le Dashboard. Son état actif confirme son enregistrement et sa communication avec le serveur central. Cette vérification est nécessaire pour interpréter les essais suivants : une absence d’alerte n’aurait pas le même sens si l’agent était déconnecté. Elle ne suffit cependant pas à valider tous les fichiers journaux, tous les modules ou toutes les règles de détection.

## Essai local : changement d’identité avec `su`

J’ai utilisé un compte local de test disposant de faibles privilèges, puis tenté de changer d’identité vers le compte administrateur avec des informations incorrectes. Les échecs étaient volontaires : je cherchais à observer leur trace et leur traitement dans le SIEM, sans obtenir un accès privilégié.

Les événements présentés dans la capture correspondent aux règles **5301**, **5503** et **5557**, de niveau **5**. Ils concernent l’échec du changement d’identité, un refus d’authentification PAM et une vérification de mot de passe échouée. Je retiens surtout que plusieurs messages peuvent décrire différentes étapes d’une même tentative. Il serait donc incorrect d’assimiler automatiquement chaque événement à une attaque distincte.

Le résultat observable est la remontée d’échecs d’authentification. Aucune élévation de privilèges réussie n’est démontrée. Le niveau 5 correspond à la classification des règles Wazuh, pas à une preuve d’incident critique. Voir la [classification officielle](https://documentation.wazuh.com/current/user-manual/ruleset/rules/rules-classification.html) et les [règles PAM](https://github.com/wazuh/wazuh/blob/master/ruleset/rules/0085-pam_rules.xml).

## Essai SSH : tentatives automatisées

Une capture Hydra montre un essai local contre le service SSH de la machine, à l’adresse de boucle locale, avec un utilisateur fictif et huit entrées de test. L’outil indique **zéro mot de passe valide trouvé**. Ce résultat décrit un test d’authentification infructueux ; il ne démontre pas un accès à la machine.

Une autre capture présente des événements **5710**, associés à des tentatives SSH utilisant un compte inexistant. Les horaires affichés ne permettent pas d’attribuer formellement ces événements à l’exécution Hydra conservée. Je les présente donc comme deux observations complémentaires, sans affirmer une relation causale établie.

La règle 5710 décrit un événement individuel. La validation d’une corrélation de répétitions, telle que celle de la règle 5712, demanderait un essai dédié et une alerte explicitement identifiée. Cette validation n’est pas démontrée ici. Les [règles SSH officielles](https://github.com/wazuh/wazuh/blob/master/ruleset/rules/0095-sshd_rules.xml) permettent de distinguer ces mécanismes.

## Preuves disponibles

| Preuve | Ce qu’elle permet de vérifier |
|---|---|
| [Agent actif](../assets/agent-actif.jpeg) | Enregistrement et connexion de l’agent |
| [Alertes PAM](../assets/alertes-pam.jpeg) | Événements 5301, 5503 et 5557 |
| [Alertes SSH](../assets/alertes-ssh.jpeg) | Événements 5710, sans preuve de compromission |
| [Vue des authentifications](../assets/vue-authentifications.jpeg) | 83 événements au total, 52 échecs et un succès sur 24 heures |

Les compteurs agrégés couvrent une fenêtre de collecte. Je ne les attribue pas à une seule simulation. Pour approfondir ce travail, je prévoirais des essais horodatés séparément, des filtres par agent et des exports de résultats. Ces améliorations restent à réaliser.
