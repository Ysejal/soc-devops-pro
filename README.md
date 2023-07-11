% title
% Soufiane CHIKAR; Wissam Elaouni
# Projet SIRP

## Introduction

### Résumé du projet à réaliser

Le but de ce projet est de disposer d'une plateforme SIRP avec des logiciels open source qui puisse être évolutif et hautement disponible à des fins pédagogiques dans un environnement de simulation mais qui, pourvu des ressources matérielles nécessaires, puisse être implémenté pour la gestion d'incidents de toute nature ou organisation qui a besoin de mettre en place ce type de solution dans le cadre de son SMSI.

Sur le plan méthodologique, les étapes suivantes ont été suivies dans ce travail :

- Analysez les fonctionnalités nécessaires et voir quelles options sont disponibles

- Choisir les composants et concevoir une solution en fonction de l'étude réalisée et du périmètre proposé

- Mettre en œuvre et intégrer les différents composants/produits qui composent la solution

- Réaliser des tests, des conclusions et documenter les améliorations possibles

- Compléter la documentation et la présentation sur le travail effectué

En conséquence, nous avons une plateforme SIRP opérationnelle qui permet de gérer les incidents depuis leur détection jusqu'à leur résolution, avec leur documentation correspondante pour les incidents possibles et leur clôture, qui permet de partager des informations (IoCs) avec des tiers si nécessaire, et qui utilise des sources externes d'intelligence pour aider les analystes de sécurité dans ce qui peut être le quotidien de toute entreprise ou organisation, le tout en utilisant les technologies cloud actuelles pour sa mise en œuvre et sa gestion.

### Contexte

Dans le contexte actuel, les entreprises et organisations s'engagent dans un processus de numérisation (Cloud Computing) rapide pour continuer à offrir leurs services en ligne. La maturité numérique varie, certaines entreprises étant plus avancées que d'autres. Avec la numérisation croissante, la cybersécurité devient essentielle pour se protéger des cybers incidents.

La résilience en matière de cybersécurité repose sur la détection, la contention et la réponse aux incidents. Les plateformes SIRP (Security Incident Response Platform) aident à gérer les incidents de cybersécurité.

Ce projet vise à sélectionner et simplifier la création de telles plateformes pour les déployer soit sur site, soit dans le cloud, ou sous forme de SOC as a Service. La collaboration et le partage d'informations entre les "bons acteurs" sont cruciaux pour lutter contre les cybercriminels qui disposent de plus en plus de ressources et collaborent entre eux.

## Synthèse des outils et technologies utilisés

Afin d'avoir une plate-forme de gestion des incidents opérationnelle, nous avons besoin d'éléments pour détecter les menaces ou les attaques possibles, nous devons avoir un espace pour stocker les événements générés par les outils de détection où nous pouvons corréler, analyser et rechercher des modèles, un endroit où pour pouvoir observer les informations des différentes sources d'événements à surveiller, nous devons pouvoir générer des alertes sur la base de ces informations et gérer de manière centralisée ces éventuels incidents, afin que tous les participants, qu'ils soient techniciens informatiques, analystes de sécurité ou gestionnaires, peuvent partager des informations et enrichir un dossier ouvert, ainsi que pouvoir partager ces informations entre toutes les parties intéressées par le processus.

Pour pouvoir réaliser toutes ces activités, une série d'outils open source ont été sélectionnés qui permettent d'apporter des solutions aux différentes étapes de la gestion d'un incident et que nous pouvons intégrer dans notre solution SIRP.

### Détection et réponse

WAZUH est ce qu'on appelle un HIDS, il gère la partie serveur qui s'intègre à ELK et les agents qui sont installé sur les ordinateurs à protéger (Windows et Linux). Il sera intégré à la solution pour faire partie du système de détection.

L'IPS est un système qui permet la protection des ordinateurs au niveau du réseau, c'est un NIDS qui devraient donc permettre de détecter les schémas d'attaque au niveau du réseau et donc ferait partie de la stratégie de détection (que WAZUH ne peut pas couvrir, car il y a éléments de réseau dans lesquels les agents ne peuvent pas être disponible). Cela ferait également partie de notre système de réponse, puisque l'IPS a la capacité de réagir en bloquant certains types d'attaques. Il s'agit de l'utiliser comme une autre source d'ingestion d'informations de sécurité dans notre ELK.

Les deux produits open source les plus connus qui couvrent les fonctionnalités susmentionnées sont SNORT, qui est un projet avec une longue histoire, et Suricata, qui est beaucoup plus moderne, mais qui reçoit chaque jour plus de soutien de la part de la communauté.

Le pare-feu est un élément de sécurité traditionnel qui ferait partie du minimum requis dans toute installation d'une entreprise ou d'une organisation. Il peut fournir certaines capacités en termes de détection au niveau du réseau, dans notre cas nous utiliserons la couche 4 du modèle OSI. Il peut également faire partie du système de réponse à certains types d'incidents.

Nous avons actuellement iptables et firewalld disponibles dans la plupart des distributions Linux. Le premier est assez ancien, bien qu'il existe encore des versions qui l'utilisent, tandis que le second est plus moderne et est celui choisi par les dernières versions de la plupart des distributions commerciales (RHEL). Ce sont des pares-feux à états, ce qui signifie qu'ils analysent les paquets pour appliquer des règles qui tiennent compte de l'état des connexions.

WAF est un composant de détection et de réponse, il s'agit d'un pare-feu de couche 7 OSI, appelé pare-feu applicatif et servant à protéger les applications Web de certains types de menaces (comme le Top 10 OWASP). Nous pouvons utiliser mod_security, qui est un logiciel open source qui permet l'intégration avec les serveurs Web Apache et NGINX les plus connus et les plus utilisés.

Le proxy est un serveur intermédiaire entre les utilisateurs et les ressources Internet, qui peut être utilisé pour collecter des informations sur l'activité des utilisateurs. Ces informations peuvent ensuite être transmises au SIEM pour analyse. De plus, le proxy peut également être utilisé pour bloquer l'accès à des URL malveillantes détectées par le SIEM.

Sysmon est un service gratuit du système Windows (qui est inclus dans la suite d'outils Sysinternals), ainsi qu'un pilote de périphérique qui, une fois installé sur un système, reste actif lors des redémarrages du système pour surveiller et enregistrer l'activité du système dans le journal des événements de Windows. Il fournit des informations détaillées sur les créations de processus, les connexions réseaux. En collectant les événements qu'il génère à l'aide de la collecte d'événements de Windows ou d'agents SIEM, puis en les analysant ultérieurement, il est possible d'identifier l'activité malveillante ou anormale et de comprendre comment les intrus et les logiciels malveillants opèrent sur votre réseau (il fournit des informations d'événements, mais des alertes doivent être définies dans le SIEM pour qu'il fonctionne comme un système de détection).

Nous pouvons voir une analyse de base initiale de produits, de composants et de fonctionnalités qui peuvent être candidats à faire partie de la solution à mettre en place.

### Collecte, indexation et stockage d'informations et d'événements

ELK est la base de notre SIEM, il s'agit de trois produits open source qui fonctionnent généralement avec la Suite Elastic, c'est pourquoi ils sont connus par leurs acronymes unifiés. On parle d'Elasticsearch, Logstash et Kibana. C'est une plate-forme qui nous permet d'acquérir une observabilité de tout ce qui se passe dans notre environnement, en l'enrichissant d'informations provenant de diverses sources et en améliorant ainsi la sécurité.

La fonctionnalité de chacun des composants est brièvement décrite :

Avec Logstash, vous ingérez, transformez et transférez vos données sans plus vous soucier de leur format ou de leur complexité. Transformez des données non structurées en données structurées via grok, déchiffrez des coordonnées géographiques à partir d'adresses IP, anonymisez ou excluez les champs confidentiels, et facilitez-vous tout le processus de traitement.

Elasticsearch est un moteur de recherche et d'analyse distribué RESTful (utilisant l'API REST standard) capable de traiter un nombre croissant de cas d'utilisation (dans notre cas, utilisé comme SIEM). En tant que cœur de la Suite Elastic, il stocke de manière centralisée les données pour la recherche.

Kibana est une interface utilisateur open source qui vous permet de visualiser les données stockées par Elasticsearch. Il existe des modèles pour pouvoir configurer des tableaux de bord de recherche par défaut (par exemple l'intégration Wazuh).

Filebeat est un agent qui surveille les fichiers de Logs et les événements configurés sur un ordinateur pour les transmettre à Elasticsearch ou Logstash pour indexation.

Packetbeat est un élément qui fait aussi partie de ce qu'ils appellent ELK Beats et qui permet de surveiller le réseau, c'est un analyseur de paquets qui envoie des informations d'un hôte ou d'un conteneur vers Logstash ou Elasticsearch.

Elastalert est un outil simple pour alerter sur les anomalies, les pics ou d'autres modèles d'intérêt dans les données Elasticsearch. Il fonctionne en combinant Elasticsearch avec deux types de composants, les types de règles et les alertes. Elasticsearch est interrogé périodiquement et les données sont transmises au type de règle, qui détermine quand une correspondance est trouvée. Ce composant fonctionne avec Elasticsearch mais ne fait pas partie de la Suite Elastic. Praeco est également mentionné pour pouvoir implémenter les alertes graphiquement, cet outil fonctionne avec Elastalert.


## Gestion des incidents et partage d'informations

TheHive est un composant de base de la plateforme SIRP, il se définit comme une solution open source évolutive conçue pour la gestion des incidents de sécurité de l'ouverture à la fermeture. Même s'il faut dire que si nous n'avions que ce composant, tout serait manuel et en fait nous n'aurions pas de capacités de détection ou de réponse, ce serait une solution incomplète.

Pour compléter TheHive on va donc utiliser Cortex, c'est une plateforme d'analyse de sécurité open source qui aide les équipes de sécurité à automatiser et à coordonner les tâches de réponse aux incidents de sécurité, en fournissant une interface conviviale et des connecteurs pour intégrer les outils de sécurité existants.

MISP est une plate-forme de partage (Wiki) de menaces open source maintenue par CIRCL qui, parmi de nombreuses autres utilisations, permet à l'opérateur de s'abonner à des flux de renseignements sur les menaces. Ces flux peuvent être des abonnements payants ou des flux gérés par la communauté de diverses organisations (telles que d'autres sociétés ou des organismes officiels tels que l'ANSSI). Il permet de partager ce que l'on appelle des IoC afin qu'un incident puisse être évité ou traité en créant des règles de détection ou de blocage basées sur des modèles.

## Analyse et conception de la solution

La gestion des incidents est un processus d'amélioration continue qui commence par la phase de préparation et de prévention. Cette phase nécessite des outils de détection et de surveillance adaptés aux besoins de l'organisation. Les systèmes d'exploitation et les middleware ont des journaux de sécurité, mais des outils supplémentaires sont nécessaires pour détecter les activités malveillantes. Les RBS (Endpoint Detection and Response) combinent diverses fonctionnalités de sécurité et incluent généralement un antivirus et un HIDS.

Un autre élément important est un NIDS ou NIPS pour la détection et la réponse au niveau du réseau. Les RBS fonctionnent au niveau de l'hôte tandis que les NIDS/NIPS fonctionnent au niveau du réseau pour combler les lacunes de détection. Les solutions open source telles que MISP sont des exemples populaires de produits de détection au niveau du réseau. Dans l'ensemble, il est crucial de disposer des outils adéquats pour détecter et répondre aux menaces de sécurité afin de protéger efficacement une organisation.

### Wazuh

Comme indiqué précédemment, Wazuh est une solution open source qui nous permet de surveiller nos appareils afin de détecter les menaces, l'intégrité de nos systèmes et de réagir aux incidents, entre autres (comme « compliance » pour vérifier que les ordinateurs respectent la politique de sécurité de l'entreprise).
Pourquoi Wazuh ?
Bien qu'il puisse y avoir d'autres options, il s'agit d'une suite qui intègre de multiples fonctionnalités qu'il serait autrement nécessaire de déployer avec différents composants logiciels et que nous n'aurions donc pas l'intégration que nous pouvons avoir avec ce produit. Il s'agit d'une solution avec un support communautaire suffisant (de nombreuses solutions apparaissent et disparaissent en peu de temps) et suffisamment mature pour être implémentée dans une organisation, ce qui est réalisé en publiant de nouvelles versions en dépannant les bugs et en ajoutant des fonctionnalités au produit (au moment de réaliser cette étude du produit, ils vont dans la version 4.4.1).
Outre la communauté, il existe également une entreprise qui peut prendre en charge le produit, point important dans de nombreuses organisations sans lequel ils ne se lanceraient pas dans la mise en œuvre de cette solution, la société qui prend en charge le produit Wazuh Inc. 

Deux services d'assistance sont disponibles :

•   Standard : 8x5 pendant les heures de bureau (réponse maximale dans 8 heures)
•   Premium : 24x7 (réponse maximale en 4 heures)
