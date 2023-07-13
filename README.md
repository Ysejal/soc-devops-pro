# Projet SIRP
Etudiant : Soufiane CHIKAR; Wissam Elaouni
---

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

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/9b78d9af-fd5a-4504-b48a-44caa62d0f6c)

Illustration 1 - Fonctionnalités de l'agent Wazuh

Ils ont également la possibilité d'être consommés en mode SaaS en souscrivant ce service à Wazuh Inc.
Il s'agit d'un produit évolutif qui permet de le déployer dans une organisation quelle que soit sa taille à l'aide d'une structure client/serveur, les ordinateurs à protéger auraient la partie cliente dans ce cas (Wazuh Agent) et nous pouvons avoir un ou plusieurs serveurs (Wazuh Server), ils peuvent évoluer horizontalement pour recevoir des informations des agents, ainsi que pour leur configuration.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/e18fb7d8-6bb0-4bb8-83d2-a478de6a73bf)

Illustration 2 - Fonctionnalités Wazuh Server

Il s'agit d'une plate-forme multiplateforme, ce qui est également important car les organisations peuvent être très hétérogènes à cet égard, bien que la plate-forme utilisateur prédominante soit généralement Windows, il y a aussi ceux qui choisissent Mac ou Linux, la partie serveurs ouvre encore plus l'éventail des possibilités (bien qu'aujourd'hui nous ayons principalement Linux et Windows), nous pouvons avoir notre Wazuh Agent à la fois sur Windows, Linux, Mac OS, AIX, Solaris et HP-UX.

Vous avez plusieurs options de déploiement sur des solutions de type IaaS ou CaaS, dans le premier cas vous avez plusieurs options d'installation sur un seul serveur avec un OVA, nous avons le système d'exploitation et le logiciel préinstallé, est une solution rapide pour travailler immédiatement avec le produit, bien que non recommandée pour une organisation de taille moyenne/grande en fonction des performances, des instructions sont également disponibles pour l’installation de ses composants de manière distribuée et avec des automatismes puppet et ansible.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/589e5554-bf6a-4c28-9af2-2d79f4edfc61)

Illustration 3 - Installation distribuée Wazuh

En ce qui concerne son déploiement sur CaaS, nous pouvons le faire via Docker et Kubernetes, c'est donc une solution très flexible et qui s'adapte très bien au paysage des déploiements actuels de systèmes sur site au sein de l'organisation, dans les clouds publics, privés ou hybrides.

Une autre option pour laquelle cette solution est attrayante est par son intégration avec Kibana un composant de l'Elastic Stack dont nous parlerons plus tard, ce qui nous permet de visualiser les informations générées par le produit sans avoir à consacrer un temps supplémentaire à ce point.

Enfin, il s'agit d'une solution qui doit nous aider à assurer la sécurité d'une organisation, mais elle est également une solution qui met en œuvre les fonctions de sécurité dans son fonctionnement, qui peut normalement nécessiter une organisation, des communications sécurisées, une authentification, etc.

### Capacités Wazuh

Voici un petit aperçu des principales fonctionnalités de Wazuh.

Les événements générés par les agents Wazuh sont envoyés au serveur Wazuh où vous pouvez définir des alertes comme des alertes en fonction de leur gravité, par défaut à partir du niveau 3 une alerte est ignorée.
Les agents ont également la capacité d'effectuer des réponses actives, le produit est livré avec quelques scripts pour effectuer certaines tâches de réponse telles que le blocage d'un accès avec le pare-feu local ou la création d'un chemin nul.

Il est possible d'effectuer des tâches avec Wazuh sur des ordinateurs sans agent en utilisant la connexion SSH, tels que des routeurs, des pare-feu ou des commutateurs.
Les règles de détection disponibles pour le produit sont gérées de manière centralisée à partir d'un référentiel Github et doivent être configurées pour être mises à jour en cas d'apparition de nouvelles règles ou vous pouvez ajouter vos propres règles pour améliorer les capacités de détection.

Le produit peut être intégré par API avec d'autres systèmes :
•      Slack : vous pouvez créer des messages dans slack pour publier des alertes
•      PagerDuty : est un service SaaS pour incident response dans lequel vous pouvez créer un service pour publier des alertes dans votre Incident Dashboard
•      VirusTotal : peut être intégré pour permettre la détection de fichiers malveillants à l'aide du BDD de VirusTotal
•      Custom Integration : utilisé pour l'intégration avec des produits tiers, il s'agit d'une option générique qui permet l'intégration par script

Il est possible d'accéder à Wazuh Server via le Web avec un navigateur ou d'interagir avec celui-ci via l'API Wazuh, une API RESTful qui permet de gérer le Wazuh, ainsi que d'effectuer diverses requêtes.

L'intégration avec Kibana, comme indiqué ci-dessus est l'une des fonctionnalités de ce produit, dispose d'un plugin qui permet aux utilisateurs de visualiser et d'analyser les alertes Wazuh stockées sur Elasticsearch. Les utilisateurs peuvent obtenir des statistiques par agent, rechercher des alertes et les filtrer en utilisant différentes vues. S'intègre à l'API Wazuh pour récupérer des informations sur la gestion et la configuration des agents, des journaux, des règles définies, des groupes, etc.

Parmi les autres fonctionnalités du produit, citons l'analyse des vulnérabilités basée sur les BDD CVE et la détection du logiciel installé, le Wazuh fait également l'inventaire, ainsi que le scan de compatibilité des stratégies et des paramètres de sécurité de l'ordinateur.

### Filebeat

Il s'agit d'un agent léger qui vous permet de transférer et de centraliser des données de journaux. Il surveille les fichiers configurés et permet de les envoyer à Elastichsearch ou Logstash pour indexation.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/87d894a9-07f7-4983-9bb9-78613adff3d6)

Illustration 4 - Surveillance et envoi des journaux

Pour collecter ces journaux, vous devez installer cet agent sur chacun des ordinateurs que vous voulez surveiller et envoyer des journaux à notre SIEM et configurer les fichiers à partir desquels vous voulez recueillir des informations.

Vous devez également indiquer le format de ces journaux, afin que vous puissiez « couper » les informations de sorte que ces fichiers puissent ensuite être exploités par le biais de requêtes.

Nous disposons de versions pour Windows et Linux, la dernière version disponible est la version 7.12 au moment de l'exécution de ce travail d'analyse. Il peut également être installé sur Docker.

### Winlogbeat

Son rôle est similaire à celui de l'agent Filebeat que nous avons vu au point précédent, bien qu'il s'agisse ici d'un composant spécifique de systèmes Windows, car il vous permet de collecter et d'envoyer les informations générées dans le journal des événements de ce S.O.
Bien qu'il puisse être utile de disposer de tous les événements Windows centralisés dans Elastic dans notre cas, nous nous concentrerons sur ceux du journal des événements de sécurité.

### Auditbeat

Cet agent est installé sur les plates-formes Linux et vous permet d'extraire des informations des librairies Audit de ce S.O. sans avoir à modifier la configuration du démon audited.

De cette façon, nous pouvons envoyer des données sur les actions réalisées par les utilisateurs sur le S.O. Linux à la plateforme SIEM.

### Packetbeat

Il s'agit de la dernière des « beats » à analyser pour notre plateforme SIRP en tant qu'élément de collecte d'informations réseau.
Cet agent vous permet de surveiller le trafic réseau et d'analyser en temps réel les paquets envoyés ou reçus par un hôte ou un conteneur et de les envoyer à Logstash ou Elasticsearch.
Cette fonctionnalité peut être très utile sur notre plate-forme si nous ne pouvons pas avoir un NIDS ou un NIPS dans tous les emplacements réseau de notre organisation et peut nous permettre d'analyser le trafic réseau entre les ordinateurs de notre réseau local, trafic que nous ne ferons généralement pas passer par un NIDS ou un NIPS.
Packetbeat peut analyser les paquets et décoder les données au niveau de l'application HTTP, DNS, etc. en donnant quelques exemples de protocoles de couche 7.

### Logstash

C'est un composant de la suite de produits Elastic qui permet la réception de journaux, le sablage et la transformation en temps réel, indépendamment de leur format ou de leur complexité. Dans notre cas, il est utilisé pour envoyer les données à Elasticsearch, mais la sortie de données peut être autre, envoi par courrier, bbdd, etc.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/604461db-e734-440b-a487-b0cb0b1dca69)

Illustration 5 - Billets / Sorties Logstash

Installable sur Linux, Windows, MacOS ou dans Docker

### Fonctionnalités Logstash

Entrées : Entrées facilement à partir de vos journaux, métriques, applications Web, entrepôts de données.
Filtres : vous pouvez configurer des filtres personnalisés, même si le produit dispose déjà d'une bibliothèque de modèles.

•      Dérive la structure à partir de données non structurées avec grok

•      Décrypte les coordonnées géographiques à partir des adresses IP

•      Il anonymise les données PII et exclut totalement les champs sensibles

•      Facilite le traitement global, quelle que soit la source de données, le format ou le schéma.

Sorties : Elasticsearch est la base de la suite de produits pour la recherche et l'analyse de données, mais ce n'est pas la seule option possible. Logstash dispose d'une variété de sorties qui vous permettent d'acheminer les données où vous le souhaitez, ce qui vous donne la possibilité de déverrouiller un grand nombre de cas d'utilisation ultérieurs.

### Elasticsearch

C'est le noyau de la suite, le composant central de l'outil. C'est un moteur de recherche et d'analyse distribué avec une API RESTful.

L'une de ses principales fonctionnalités est de faciliter la recherche et dispose d'un indexeur qui vous permet d'organiser les informations de manière à pouvoir répondre rapidement aux requêtes que vous effectuez.

Conçue pour prendre en charge de gros volumes de données, c'est une solution évolutive que vous pouvez créer un cluster qui vous permet d'ajouter des nœuds pour une plus grande capacité de traitement des données.

L'outil est centré sur la recherche, il permet des recherches structurées et non structurées.

Actuellement, la dernière version disponible du produit est la version 7.12 et peut être installée sur Linux, Windows ou MacOS, ainsi que sur la technologie de conteneur Docker.

### Fonctionnalités d'Elasticsearch

Il permet la gestion des nœuds dans « Data tiers », il est possible de configurer les nœuds en fonction des performances des nœuds dans trois catégories Hot, Warm et Cold et il permet d’automatiser le déplacement des données entre les nœuds, bien que nous ne l’utilisions pas pour ce module de gestion du trafic sécurisé, dans un véritable SIEM, nous pourrions enregistrer les journaux les plus récents sur des nœuds de type Hot, où la recherche doit être en temps réel et où les données les plus anciennes pourraient être reléguées à des nœuds de stockage à faible coût de type Cold, par exemple, ce qui peut être nécessaire dans les organisations qui ont besoin de stocker des journaux pendant de longues périodes en raison d’exigences légales.

De nombreuses fonctionnalités du produit sont disponibles pour les duplications, snapshots, etc., en tant que bon produit à utiliser dans un environnement d'entreprise sérieux, vous avez de multiples options pour maintenir les performances et la disponibilité, nous ne traiterons pas ces fonctionnalités ici car l'utilisation du produit que nous ferons dans ce TFM ne fera que gratter la surface.

Il dispose également d'un ensemble de fonctionnalités de sécurité, nécessaires pour ce type de produits qui peuvent contenir des données sensibles, l'une des choses de base est qu'un bon contrôle d'accès est nécessaire (l'outil fournit des mécanismes RBAC et ABAC pour affiner l'accès aux différentes ressources stockées dans Elastic) et enregistre un journal d'audit des accès corrects ou erronés pour pouvoir détecter les attaques. Un autre point important est le cryptage des données, si possible dans les communications avec Elasticsearch, ce qui serait le cryptage des données locales qui serait délégué au S.O. des nœuds.

Une autre fonctionnalité de sécurité disponible pour le produit est le filtrage IP, vous pouvez appliquer le filtrage IP aux clients d'application, aux clients de nœud ou clients de transport, ainsi que d'autres nœuds qui tentent de joindre le cluster Elastic.

En ce qui concerne l'authentification, il existe plusieurs méthodes pour la gérer : native, LDAP, SAML, etc. et permet d'utiliser SSO avec Kibana (il n'est pas nécessaire d'utiliser différents utilisateurs/passwords lors de l'accès via Kibana ou directement à Elasticsearch).
Elasticsearch permet de définir des alertes et dispose d'un large éventail de notifications possibles : email, webhooks, IBM Resilient, Jira, Microsoft Teams, Slack, etc.

Le produit fournit une API RESTful et JSON puissantes, ce qui permet l'accès au produit via du code d'application dans plusieurs langages Java, Go, .NET, PHP, Javascript, Perl, Python, Ruby, etc.
Vous disposez d'autres fonctionnalités que vous pouvez consulter sur le site Web du produit, mais que nous ne détaillerons pas dans ce document.

### Kibana

Ce composant est une interface utilisateur open source qui vous permet de visualiser les données Elasticsearch de manière graphique et visuelle via votre interface Web.

Les principales caractéristiques seront examinées, bien que le produit ait de multiples possibilités d'exploitation des données, des tableaux de bord, etc. Nous ne passerons pas beaucoup de temps car, comme cela a été dit, il s'agit essentiellement d'une interface de visualisation des données.

Dans notre cas, il nous permettra de visualiser les données collectées dans Elasticsearch à partir de différentes sources et en particulier de Wazuh. Ce peut être un outil très utile pour un analyste pour trouver des motifs, des tendances ou même sa capacité à montrer des informations géospatiales (sur une carte).

Comme le reste de la suite ELK est disponible pour Linux, Windows et MacOS et la plate-forme de conteneurs Docker.

### Capacités de Kibana

Comme cela a déjà été dit, la principale capacité du produit est l'affichage des données.

•      Kibana Lens vous permet de créer des écrans de données avec drag-and-drop

•      Time series, charts, métriques, tables de données, informations géopositionnelles (représentant des données sur une carte), etc. sont des éléments que vous pouvez configurer et afficher dans Kibana

•      Créer et configurer des tableaux de bord

•      Interface console, vous permet d'utiliser du code pour interroger Elasticsearch

•      Générer des graphes d'analyse

•      Permet l'exportation de rapports en mode PDF ou image PNG

•      Exporter les informations via CSV

### Analyse des composants de gestion des incidents

Jusqu'à présent, nous avons analysé les produits qui nous permettent de détecter les menaces et, dans certains cas, de répondre de manière autonome à ce qu'un RBS et un NIPS peuvent faire, nous avons également constaté qu'ils peuvent générer des événements, des alertes et que ceux-ci peuvent être envoyés à notre plateforme SIEM, ainsi que les journaux S.O. et middleware, y compris le trafic réseau reçu par les ordinateurs.

Avec toutes ces informations au cas où une menace deviendrait un incident, nous avons besoin d'une plate-forme pour le gérer et cette plate-forme est principalement TheHive, avec l'aide de Cortex et de MISP.

### TheHive

Hive est un outil open source qui permet de gérer les incidents de sécurité, conçu pour être utilisé par les SOC, les CSIRT, les CERT, etc. en général, quiconque souhaite gérer les incidents de sécurité qui doivent faire l'objet d'une enquête.

Il s’agit d’une solution évolutive, car elle permet une croissance horizontale du nombre de nœuds pour améliorer les performances, nécessite Java pour son fonctionnement et utilise Elasticsearch comme référentiel de données.

L'outil permet de partager des informations, de s'enrichir de sources externes (MISP) et de répondre aux incidents à l'aide d'un autre composant dont Cortex parlera plus tard.

Au moment de cette étude, la dernière version disponible est la version 4.1.0 Vous pouvez obtenir des services professionnels pour l'installation, la configuration et le support du produit, ce qui est une valeur supplémentaire pour une utilisation dans les organisations.

Le produit peut être implanté sur une machine physique ou virtuelle (IaaS) ou contaminé par Docker (CaaS).

### Capacités de TheHive

Comme résumé sur le site du projet TheHive, voici les fonctionnalités de base du produit.

Collaboration :

•      Plusieurs analystes SOC et CERT peuvent collaborer simultanément sur les investigations

•      Grâce à la diffusion en direct intégrée, les informations en temps réel relatives aux cas, tâches, observables et CODES nouveaux ou existants sont disponibles pour tous les membres de l'équipe.

•      Les notifications spéciales leur permettent de gérer ou d'affecter de nouvelles tâches et d'afficher un aperçu des nouveaux événements MISP et des alertes provenant de sources multiples, telles que les rapports par e-mail, les fournisseurs CTI et SIEM. Ils peuvent ensuite les importer et les examiner immédiatement.

Élaboration :

•      Les cas et les tâches associées peuvent être créés à l'aide d'un moteur de modèle simple mais puissant.

•      Vous pouvez ajouter des mesures et des champs personnalisés à vos modèles pour stimuler l'activité de votre équipe, identifier les types d'investigations qui prennent beaucoup de temps et rechercher l'automatisation des tâches fastidieuses à l'aide de tableaux de bord dynamiques.

•      Les analystes peuvent enregistrer leur progression, joindre des preuves ou des fichiers notables, ajouter des balises et importer des fichiers ZIP protégés par mot de passe contenant des programmes malveillants ou des données suspectes sans les ouvrir.

### Action :

•      Ajoutez un, des centaines ou des milliers d'observateurs à chaque cas que vous créez ou importez directement à partir d'un événement MISP ou de toute alerte envoyée à la plateforme.

•      Classer et filtrer rapidement.

•      Exploitez la puissance de Cortex et de ses analyseurs et répondeurs pour obtenir des informations précieuses, accélérer vos recherches et contenir les menaces.

•      Tirez parti des étiquettes, marquez les IOC, les observations et identifiez les observations précédentes pour alimenter votre intelligence des menaces.

•      Une fois les enquêtes terminées, exportez les IOC vers une ou plusieurs instances de MISP.

Le produit TheHive fournit une API REST pour l'interaction et l'automatisation des tâches en plus de l'accès Web pour les utilisateurs interactifs.

### Cortex

Cortex est un analyseur d'observables et un moteur de réponse actif qui permet, en conjonction avec TheHive, d'aider à la phase de confinement d'un incident de sécurité.

L'outil est open source et permet d'automatiser le traitement des observables en exposant une REST API.
Bien que Cortex soit free, cela ne signifie pas que les analyseurs et les répondeurs qu'il inclut lors de demandes à des tiers ne nécessitent pas de paiement ou d'abonnement à l'utilisation.

### Capacités de Cortex

Cortex peut être utilisé de manière autonome en utilisant son « interface » Web ou en association avec la plateforme de gestion des incidents TheHive, comme indiqué ci-dessus, il permet d'automatiser et d'exécuter simultanément des analyseurs et des répondeurs pour plusieurs cas.


### Analyseurs/répondeurs :

•      Vous disposez d'un grand nombre d'analyseurs ou vous pouvez créer vos propres analyseurs, ainsi que des répondeurs utilisant n'importe quel langage de programmation qui peut courir sur Linux.

•      Ces analyseurs ou répondeurs peuvent être disponibles pour l'ensemble de l'équipe (cela est utile pour plusieurs raisons, chaque membre de l'équipe n'a pas à effectuer ses propres développements et peut partager des API KEYS de services externes qui peuvent être libres ou payants.

•      Permet d'interroger plusieurs instances MISP (composant que nous aborderons plus tard).

### Interaction

•      TheHive peut se connecter à une ou plusieurs instances de Cortex et peut analyser un grand nombre d'observables à la fois, ainsi que donner des réponses actives.

•      Avec le moteur de rapport TheHive, il est facile d'analyser la sortie de Cortex et de l'afficher comme vous le souhaitez.

•      Vous pouvez également utiliser Cortex comme produit autonome en utilisant votre interface utilisateur Web pour gérer plusieurs organisations, analyseurs et définir des limites de requêtes.

•      Cortex peut interagir avec d'autres produits via son API REST ou via Cortex4py.

### Exécution :

•      Cortex est livré avec plus d'une centaine d'analyseurs pour les services populaires tels que VirusTotal, Joe Sandbox, DomainTools, PassiveTotal, Google Safe Browsing, Shodan et Onyphe. Identifiez les contacts abusifs, analysez les fichiers dans différents formats tels que OLE et OpenXML pour détecter les macros VBA, générez des informations utiles dans les fichiers PE, PDF et bien plus encore.

•      Les analyseurs Cortex peuvent également être consultés à partir de MISP pour enrichir les événements et étendre la couverture de vos investigations.

### MISP

Il s'agit d'une plateforme open source pour le traitement des renseignements sur les menaces et d'une norme ouverte permettant de partager ces renseignements entre les organisations et les CERT, CSIRT, etc. respectifs.

La clé de ce produit est l'automatisation, permettant de partager les indicateurs d'engagement (IoCs) avec les différents outils des plateformes de détection et de réponse aux menaces (IDS, SIEM, etc.)

La plate-forme permet d'enregistrer de manière structurée les IoCs et de les partager avec d'autres MISP et leur facilité d'utilisation a été étendue.

Au moment de cette analyse, la plus grande version de MISP est la version 2.4, pour installer le produit, vous pouvez utiliser un OVA, installer sur un S.O. existant ou avec Docker, la documentation du produit comprend également de nombreuses options pour effectuer l'installation avec puppet, ansible, etc.

### Fonctionnalités MISP

L'une des principales réalisations de cet outil a été son utilisation généralisée et le partage des informations entre les organisations.
Les attaques perpétrées par des cybercriminels, et même encouragées ou soutenues à des degrés divers par les États, augmentent en nombre et en sophistication, ce qui exige que les organisations travaillent également ensemble pour se protéger contre ces menaces et que MISP est un outil qui encourage ce type d'interaction de manière automatisée, en partageant l'intelligence que chaque organisation obtient après avoir analysé et enquêté sur une attaque.
Sur le site de MISP, nous pouvons trouver ses fonctionnalités  :

•      Il vous permet de disposer d'une base de données d'indicateurs et d'IoCs qui vous permet de stocker des informations techniques et non techniques sur les programmes malveillants, les incidents, les agresseurs et le renseignement.

•      Corrélation automatique, recherche de relations entre les attributs et les indicateurs de programmes malveillants, les campagnes d'attaques ou les analyses. La corrélation peut également être activée ou désactivée par événement ou par attribut.

•      Fournit un modèle de données flexible dans lequel les objets complexes peuvent être exprimés et liés pour exprimer l'intelligence des menaces, des incidents ou des éléments connectés.

•      Facilite l'échange de données en utilisant différents modèles de distribution. MISP peut synchroniser automatiquement les événements et les attributs entre différents MISP. Vous pouvez utiliser des mécanismes de filtrage pour que chaque organisation décide qu'elle partage et non pas.

•      Il fournit une interface utilisateur intuitive permettant aux utilisateurs finaux de créer, mettre à jour et collaborer sur des événements et des attributs/indicateurs. Une interface graphique pour naviguer sans problème entre les événements et leurs corrélations.

•      Fournit une fonctionnalité de graphe d'événements pour créer et afficher les relations entre les objets et les attributs. Fonctions de filtrage avancé et liste d'avertissements pour aider les analystes à contribuer aux événements et aux attributs.

•      Stockage des données dans un format structuré (qui permet l'utilisation automatisée de la base de données à diverses fins)

•      Exporter les informations pour générer des IDS (Suricata, Snort et Bro sont pris en charge par défaut), OpenIOC, texte brut, CSV, MISP XML ou sortie JSON pour l'intégration avec d'autres systèmes (IDS réseau, IDS hôte, outils personnalisés)

•      Importation de données en bloc, importation par lots, importation de texte libre, importation à partir d'OpenIOC, GFI sandbox, ThreatConnect CSV ou format MISP, plusieurs options sont proposées. Outil flexible d'importation de texte libre pour faciliter l'intégration de rapports non structurés dans MISP.

•      Échange de données : échange automatique et synchronisation avec d'autres parties et groupes de confiance à l'aide de MISP.

•      Outil flexible permettant d'importer et d'intégrer dans MISP toute source de renseignement ou OSINT tierce. MISP inclut déjà de nombreuses sources en entrée.

•      Délégation de partage : permet un mécanisme pseudo-anonyme simple pour déléguer la publication d'événements / indicateurs à une autre organisation (cela peut être utile pour ne pas révéler quel type d'attaques subit votre organisation, peut être des informations sensibles et donc ne pas être partagé).

•      API flexible permettant d'intégrer MISP à d'autres outils internes. Une bibliothèque Python appelée PyMISP est incluse pour rechercher, ajouter ou mettre à jour des attributs d'événement, gérer des échantillons de programmes malveillants, etc.

•      Classification personnalisable ou utilisation de classes définies. La taxonomie (classification) peut être locale pour votre MISP, mais elle peut également être partagée entre les instances de MISP. MISP est livré avec un ensemble prédéfini de taxonomies et de schémas de classification pour prendre en charge une norme telle que ENISA, Europol, DHS, CSIRT ou beaucoup d'autres organisations.

•      Vocabulaires de renseignement appelés MISP galaxy et inclus avec les acteurs de menaces existants, les logiciels malveillants, RAT, ransomware ou MITRE ATT & CK qui peuvent être liés à des événements dans MISP.

•      Prise en charge des observations des organisations sur les indicateurs et les attributs partagés. Vous pouvez contribuer avec des observables via l'interface utilisateur MISP, via des API comme document MISP ou des documents d'observation STIX.

•      Prise en charge STIX : exportation de données au format STIX (XML et JSON) au format STIX 2.0.

•      Chiffrement intégré et signature de notification via PGP ou S/MIME selon la configuration de l'utilisateur.

•      Canal de publication-abonnement en temps réel au sein de MISP pour obtenir automatiquement toutes les modifications (par exemple, les nouveaux événements, les indicateurs, les notifications ou le balisage).

Dans notre installation, nous configurerons MISP pour importer des informations, car nous n'aurons pas de cas à partager, bien que vous puissiez effectuer une configuration en simulant un environnement de travail réel dans lequel nous pouvons contribuer à la communauté.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/780c387b-d8b0-45ca-9fec-6439acd03c8a)

Illustration 6 - Schéma MISP

### Éléments qui composent notre plate-forme SIRP

Après avoir analysé les différents composants qui pourraient faire partie d'une solution SIRP et qui ont été traités dans les sections précédentes, il est temps de choisir ce que nous allons mettre en œuvre dans notre solution et pourquoi ces composants et pas d'autres.

Tout d'abord, nous définirons les cas d'utilisation que notre plate-forme aura, ce qui facilitera le choix des composants, cela ne signifie pas que la plate-forme ne pourra pas répondre à d'autres cas d'utilisation, mais ils ne seront pas traités dans ce TFM.

### Cas d’utilisation

Les cas d'utilisation proposés pour ce TFM consistent en plusieurs caractéristiques de gestion des incidents qui testent les différents éléments de notre plateforme.

1)    Gestion d'un incident avec des logiciels malveillants détectés par l'EDR, signalés au SIEM, en créant un cas, en enquêtant sur celui-ci et en ajoutant les informations supplémentaires nécessaires jusqu'à sa résolution

2)    Gestion d'un incident avec des menaces détectables par le réseau avec notre NIPS, signalé au SIEM, création d'un dossier, enquête, ajout d'informations le cas échéant et réponse

3)    Gestion d'un incident avec des informations sur les événements recueillis par le SIEM, création d'un dossier, enquête, ajout d'informations le cas échéant et réponse

L'idée est de pouvoir gérer les incidents à l'aide des outils indiqués, avec un enrichissement des informations provenant de sources externes et, si possible, avec des réponses automatisées.

Pour la présentation et la défense du TFM, un des trois cas d'utilisation est choisi pour montrer le travail effectué et la façon dont il se comporte dans un environnement de test généré à l'utilisation et qui est documenté à partir du point 3.

### Composants de la solution

Il s'agirait des composants de notre plateforme SIRP, qui comprend, comme déjà traité, les éléments de détection et de réponse, la gestion des événements et les journaux de sécurité, ainsi que la gestion des incidents avec intégration avec des automatismes de traitement des observateurs d'incidents, ainsi que les réponses aux incidents pour améliorer la phase d'endiguement.
Nous incluons également un élément qui nous permet de partager et de collaborer avec d'autres organisations avec nos incidents analysés, déjà étudiés et travailler et aussi enrichir nos cas avec les informations que nous pouvons recueillir.

•      Wazuh

•      Sysmon

•      Suricata

•      ELK

•      Elastalert

•      TheHive

•      Cortex

•      MISP

En ce qui concerne certaines décisions qui ont été prises pour choisir les composants, dans le cas du NIPS, Snort et Suricata ont été étudiés et on pourrait dire qu'ils sont les leaders de cette fonctionnalité dans le logiciel open source.

Suricata a été choisi pour la mise en œuvre pour plusieurs raisons, c'est un logiciel plus récent et il a un design de code plus moderne (il est né multithread), ce qui améliore généralement les performances. Selon sa documentation, Snort semble fonctionner en mode multithread, à partir de la version 3 (il était auparavant possible de l'utiliser de cette manière en démarrant plusieurs instances), un paramètre est maintenant intégré pour cela.

Les règles de Snort peuvent également être utilisées dans Suricata, dans ce type de logiciel en plus des capacités propres du produit, sa capacité de détection dépend de ses règles pour être meilleur dans la détection des menaces.
D'un autre côté, je l'ai également choisi pour des références d'intégration de Wazuh, comme de MISP.

Elastalert a également été inclus mais n'a pas été expliqué en détail dans ce chapitre pour la génération d'alertes et la création de cas dans TheHive, bien que l'exigence étant la génération de cas pour l'enquête sur les incidents dans TheHive il soit possible d'utiliser également l'option qui inclut ce produit TheHive4py, donc nous avons temporairement inclus le composant, mais nous verrons finalement quelle est la meilleure option lors de la phase de mise en œuvre et d'intégration.

Les autres composants de la solution ont été choisis en fonction des cas d'utilisation qu'il est proposé de traiter avec cette plate-forme SIRP.

### Conception de la solution

La solution pour notre plate-forme SIRP consiste à déployer les composants mentionnés ci-dessus comme s'il s'agissait d'un service de Cloud computing (SOCaaS), il s'agirait de fournir à nos clients potentiels une plate-forme complète de gestion des incidents de sécurité sur laquelle ils pourraient travailler en tant qu'utilisateurs ou déléguer entièrement à nous (votre fournisseur de services SOC et la plate-forme associée) pour effectuer toutes les tâches.

Dans ce cas, et pour simuler un environnement réel, nous aurons besoin d'éléments supplémentaires qui n'ont pas été mentionnés précédemment parce qu'ils ne font pas partie de la solution SIRP en tant que telle, comme la nécessité de disposer d'une connexion VPN entre notre site cloud et l'entreprise ou l'organisation à laquelle nous gérerons votre sécurité avec notre plateforme.

Les éléments de détection et de réponse nécessaires, tels que les agents Wazuh et les IPS, doivent être localisés sur le réseau de l'entreprise desservie par cette plateforme SIRP, ainsi que les accès nécessaires pour gérer l'installation, la réception des journaux sur la plateforme SIEM incluse dans notre solution SIRP et les autorisations permettant d'exécuter les scripts de réponse en fonction du type d'alerte détecté, le cas échéant.
Le schéma logique de la solution et de ses composants est présenté ci-dessous.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/e1966a81-5b9e-492c-ad0c-53fa00813f0f)

Illustration 7 - Schéma logique des composants

## Mise en œuvre
### Options de déploiement
 
Une fois que vous avez identifié les composants constituant la solution, vous devez maintenant prendre des décisions concernant sa mise en œuvre. Le déploiement nécessite une haute disponibilité et une solution évolutive, ce qui implique une forte automatisation des composants.

Les machines physiques ont été écartées car je ne dispose pas des équipements nécessaires pour simuler un environnement d'exploitation. La solution sera donc basée sur des machines virtuelles.

Ainsi, nous avons évalué les différentes options d'installation des produits ainsi que leurs versions. Toutes les versions ne sont pas disponibles pour une installation directe. Par exemple, les dernières versions des produits mentionnés dans le chapitre précédent de ce document nécessitent des installations manuelles en téléchargeant les produits à partir des sites Web de l'organisation qui soutient ou dirige le projet open source.

Pour donner un exemple afin de clarifier ce qui a été mentionné précédemment, la version d'ELK documentée pour l'installation avec Wazuh n'est pas celle disponible sur le site Web d'Elastic, mais plutôt l'Opendistro Elasticsearch. Cependant, le site Web de Wazuh traite également des options d'installation avec la version gratuite d'Elastic.

### Installation manuelle des produits

Les différents produits offrent des options d'installation plus ou moins documentées pour effectuer l'installation et la configuration des différents éléments à l'aide de paquets standard du système d'exploitation (SE) et/ou à partir des sources via des référentiels, principalement de type GIT.

Il existe également des OVA (Appliance Virtuelle Ouverte) avec des produits préinstallés et configurés sur un système d'exploitation Linux, comme c'est le cas du CSIRT-KIT.

Ces OVA peuvent être très utiles pour un environnement de test et pour se familiariser rapidement avec les fonctionnalités des produits (en particulier le CSIRT-KIT). Cependant, dans notre cas, nous souhaitons automatiser, assurer une haute disponibilité et permettre l'évolutivité de la solution, c'est pourquoi cette méthode a été abandonnée.

### Installation automatisée avec Ansible

Ansible est un logiciel open source de plus en plus populaire qui permet une gestion centralisée de la configuration et du déploiement des applications et des produits. Il simplifie considérablement ces tâches grâce à des recettes appelées "playbooks" (définis dans un langage déclaratif YAML) qui utilisent des modules spécifiques. Ces modules offrent des fonctionnalités telles que la gestion des packages du système d'exploitation, la copie de fichiers, et bien d'autres. Ansible dispose d'une large bibliothèque de modules et la communauté continue d'en créer de nouveaux. Des référentiels comme Ansible Galaxy offrent de nombreux exemples de playbooks et de rôles (un concept spécifique à Ansible) pour accomplir différentes tâches dans les applications et les systèmes gérés.

Dans le cas spécifique de la mise en œuvre de la plate-forme SIRP, les produits envisagés sur le site Web de Wazuh simplifient déjà la procédure, bien qu'ils nécessitent également quelques ajustements pour l'installation de Wazuh, Elastic, ainsi que l'installation des agents Wazuh et Filebeat.

Pour les autres produits choisis dans la plate-forme SIRP, il existe également des playbooks prêts à l'emploi. Comme mentionné précédemment, la communauté facilite cela en fournissant des référentiels sur des plateformes comme GIT, etc.

### Installation avec Docker / Docker Compose

Docker est l'une des plates-formes de conteneurisation les plus utilisées aujourd'hui. Elle permet de séparer les applications et l'infrastructure nécessaires à leur exécution en utilisant des microservices. Un conteneur diffère de la virtualisation traditionnelle en étant beaucoup plus léger. En effet, il ne nécessite pas le déploiement d'un système d'exploitation complet, mais seulement d'une couche système minimale et des bibliothèques requises pour exécuter l'application ou le produit. Docker facilite l'utilisation d'images pré-construites et les conteneurs sont créés, détruits et reconstruits à partir de ces images. Si une persistance des données est requise, les conteneurs doivent créer des volumes ou mapper des répertoires sur l'hôte où les conteneurs s'exécutent, afin que les données restent persistantes même si le conteneur est détruit. En termes de sécurité, les conteneurs n'exposent que les ports nécessaires à l'extérieur, tandis que les autres ports sont accessibles uniquement à l'intérieur du réseau des conteneurs (qui est généralement masqué à l'extérieur). Docker Compose est un outil qui permet d'orchestrer les conteneurs en utilisant une recette descriptive en format YAML pour définir quels conteneurs créer et quelles fonctionnalités et ressources leur assigner (notamment les réseaux et les volumes).

Dans notre cas spécifique pour la plate-forme SIRP, Docker/ Docker Compose serait une bonne solution, nous permettant ainsi d'atteindre l'objectif d'automatisation. Pour garantir une haute disponibilité et une évolutivité, il suffirait d'avoir suffisamment de matériel capable d'exécuter les conteneurs et d'appliquer la recette personnalisée pour créer et gérer les conteneurs nécessaires. Il existe des solutions basées sur des conteneurs pour Wazuh, Elasticsearch (OpenDistro et l'édition Enterprise d'Elastic), TheHive, Cortex et MISP. Dans certains cas, ces solutions ne fonctionnent pas avec les toutes dernières versions des produits, mais elles permettent d'utiliser ces images. Vous pouvez également créer votre propre image ou baser une image existante modifiée en utilisant des fichiers de configuration Dockerfile.

### Installation sur Kubernetes

À ce stade, nous disposons déjà d'une solution pour déployer automatiquement les composants d'application en adaptant les "recettes" aux différents équipements, comme mentionné précédemment. Cependant, pour une véritable orchestration des composants, nous avons Kubernetes.

Kubernetes, également connu sous le nom de k8s, est une plateforme open source portable et extensible pour la gestion des charges de travail et des services. Tout comme Docker Compose, il permet de gérer de manière déclarative le déploiement des composants (microservices) en exécutant le déploiement des PODs et des services nécessaires au bon fonctionnement de l'application ou du produit. Tout cela est rendu possible grâce à un environnement de gestion API.

Ce projet a été initié par Google en 2014 et est devenu si populaire qu'il existe plusieurs versions qui implémentent Kubernetes (kubeadm, minikube, kops, etc.). Chacune de ces versions partage des fonctionnalités de base communes, telles que les commandes, mais diffèrent dans les composants de base de la solution et leur application. Par exemple, minikube est souvent utilisé dans des environnements monomodes et/ou pour tester la technologie Kubernetes, bien qu'il soit parfaitement opérationnel.

Kubernetes est une solution extrêmement flexible qui permet la portabilité entre différents fournisseurs de Cloud public (Azure, AWS, GCP, etc.) et Cloud privé (on-premise). Il s'agit d'une plateforme conçue pour renforcer les méthodologies agiles et le DevOps, et qui prend en charge le CI/CD (Continuous Integration/Continuous Deployment).

Dans notre cas, nous utiliserons Docker comme plateforme de conteneurs, mais Kubernetes est suffisamment flexible pour être indépendant de la plateforme de conteneurs choisie (CRI, Containerd, etc.). Il propose de nombreux outils de gestion, addons, etc.

Nous avons également abordé précédemment le concept de POD, qui est l'unité minimale de Kubernetes, agissant comme une enveloppe pour un ou plusieurs conteneurs partageant certaines caractéristiques.

Maintenant que nous avons introduit la technologie, passons à notre étude de cas, qui est la plateforme SIRP avec les composants mentionnés précédemment. Il existe une documentation sur l'installation de Wazuh/Elasticsearch sur la plateforme Kubernetes. Pour les autres composants, bien qu'il puisse y avoir des solutions au sein de la communauté open source, nous pouvons nous baser sur les implémentations existantes dans Docker ou Docker Compose pour développer notre propre recette d'installation à partir des images déjà disponibles (TheHive, Cortex et MISP).

Grâce à ce système de déploiement, nous pouvons obtenir une solution avec une haute disponibilité, évolutivité et automatisation pour le déploiement et la maintenance.

### Solution d'installation choisie

Après avoir examiné les différentes options disponibles pour la mise en œuvre de notre solution SIRP, il a été décidé d'utiliser la plateforme Kubernetes, car elle offre tous les avantages nécessaires pour le déploiement de plusieurs composants.

Pour l'installation des produits de base de la plate-forme SIRP, tels que Wazuh, Elasticsearch, TheHive/Cortex et MISP, nous utiliserons Kubernetes. Quant au déploiement des agents tels que l'agent Wazuh et les différents Beats, nous utiliserons également l'outil Ansible mentionné précédemment.

Nous utiliserons donc la documentation et les procédures fournies sur les différents sites de ces projets open source, en les adaptant à notre environnement de simulation, comme décrit dans les points suivants de ce document.

Pour notre environnement Kubernetes, nous avons choisi de déployer un cluster en haute disponibilité (HA) avec Kubeadm. Le cluster sera composé de 3 nœuds maîtres et 4 nœuds de travail (tous avec l'agent kubelet installé). En terminologie Kubernetes, les nœuds maîtres (control plane) sont des nœuds contenant les composants apiserver, controller-manager, scheduler et etcd (la base de données clé-valeur qui stocke les informations du cluster, les configurations, les secrets, les services, etc.).

Les nœuds maîtres, étant les nœuds de gestion de Kubernetes, seront également utilisés pour installer Ansible.

Par mesure de sécurité, le déploiement d'applications ou d'autres composants sur les nœuds maîtres sera désactivé. Nous disposerons donc de 4 nœuds disponibles pour déployer notre solution SIRP.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/221865a7-992a-438a-985e-875d95d30ad4)

Illustration 8 - Modèle de cluster Kubernetes HA etcd stacked (kubernetes.io)

Il existe deux modèles pour créer un cluster Kubernetes HA avec Kubeadm : le modèle topologique "etcd stacked" et le modèle "etcd external".

 ### Ressources
 
Dans l'environnement de simulation utilisé dans ce projet, nous pouvons diviser les ressources utilisées en deux parties : celles qui composent le réseau d'entreprise à protéger et celles de la plateforme SIRP (simulation d'un environnement Cloud avec Kubernetes).

Toute la solution est construite autour de 4 ordinateurs physiques équipés de processeurs Intel i7 QuadCore et de 32 Go de RAM.

### Plateforme SIRP (simulation Cloud On-premise)

Les ressources utilisées dans la simulation de Cloud On-premise ou Cloud Privé sont les suivantes :

• 1 machine virtuelle - Pare-feu Pf-Sense (2 vCPU, 4 Go de RAM et de l'espace disque) - Logiciel : Appliance Pf-Sense (basée sur FreeBSD)

• 7 machines virtuelles - Cluster Kubernetes :

3 VMs nœuds maîtres (2 vCPU, 4 Go de RAM et 100 Go de disque) ou

4 VMs nœuds de travail (2 vCPU, 8 Go de RAM et 100 Go de disque)

Système d'exploitation : CentOS 8.3 ou HAProxy 1.8.23 ou Keepalived 2.0.10 ou GlusterFS 9.1 ou Docker CE 20.10.6 ou Docker-Compose 1.29.1

• Kubernetes 1.21.0 (kubectl, kubeadm, kubelet) ou Ansible 2.9.18 (nœuds maîtres uniquement)

Wazuh 4.1.1
Opendistro Elasticsearch 1.12.0 (Elasticsearch et Kibana 7.10) ou Filebeat 7.10
TheHive 4.1.4 ou Cortex 3.1.1 ou MISP 2.4.142

### Réseau d'entreprise

Les ressources utilisées dans le réseau d'entreprise sont les suivantes :

• 1 machine virtuelle - Pare-feu Pf-Sense (2 vCPU, 4 Go de RAM et de l'espace disque) - Logiciel : Appliance Pf-Sense (OVA basée sur FreeBSD)

• 1 machine virtuelle - Suricata (IPS) :

Système d'exploitation : CentOS 8.3 ou Suricata 5.0.6
Wazuh Agent 4.1.5
• 1 machine virtuelle - DNS externe, Proxy Utilisateurs, Proxy Revers Software :

Système d'exploitation : CentOS 8.3 ou Bind 9.11
Apache 2.4.37 ou Squid 4.4
Wazuh Agent 4.1.5
• 1 machine virtuelle - Serveur Web, Serveur Applicatif et Base de données :

Appliance BWA (OVA basée sur Ubuntu)
• 1 machine virtuelle - Contrôleur de domaine AD et DNS interne :

Système d'exploitation : Windows Server 2019 ou Sysmon v13.20
Wazuh Agent 4.1.5
• 1 machine virtuelle - Stations de travail :

Système d'exploitation : Windows 10
Wazuh Agent 4.1.5

### Dimensionnement des composants

Bien que cet environnement soit simulé, afin de le rendre aussi réaliste que possible et de démontrer les possibilités d'évolutivité des composants de notre plate-forme SIRP, nous avons effectué les dimensionnements suivants :

• 1 nœud Wazuh-Master et 2 nœuds Wazuh-Workers

• 3 nœuds Elasticsearch

• 1 nœud Kibana

• 1 nœud TheHive

• 1 nœud Cortex

• 1 nœud MISP

• 1 nœud Praeco-Elastalert

Dans un environnement réel, si ces ressources ne sont pas suffisantes, vous pouvez les ajuster en ajoutant des nœuds supplémentaires pour augmenter la capacité du processeur ou de la mémoire, ou en augmentant l'espace disque (en fonction du nombre d'événements à enregistrer dans Elasticsearch, du nombre d'agents, etc.).

Étant déployé sur Kubernetes, nous avons la flexibilité de jouer avec le nombre de réplicas et d'ajouter davantage de PODs à chaque déploiement.

Dans le cas de TheHive, Cortex et MISP, nous n'avons pas utilisé de réplicas pour ces produits, car selon la documentation des différents projets, en particulier pour TheHive et Cortex, ils nécessitent beaucoup de ressources. Étant donné que nous n'avions pas suffisamment de ressources matérielles disponibles, nous avons effectué une installation minimale afin de pouvoir utiliser ces produits.

### Intégration
À ce stade, nous avons installé les différents composants de notre plate-forme SIRP. Il est maintenant temps de les configurer et de les intégrer.

La configuration détaillée de chaque produit, en utilisant toutes leurs fonctionnalités, serait très longue. Par conséquent, nous avons effectué les configurations minimales nécessaires pour les rendre opérationnels et leur permettre d'effectuer les tâches requises pour le cas d'utilisation que nous présenterons ultérieurement.

### Wazuh-Elasticsearch-Kibana

Wazuh-Elasticsearch-Kibana
Une fois l'installation terminée, le produit est opérationnel. Une fois les PODs déployés, les configurations appliquées et les données initialisées, la configuration de base pour son fonctionnement est déjà implémentée, car il s'agit d'une suite de produits configurée par défaut pour être intégrée lors de l'installation et de l'activation des composants.

Wazuh peut fonctionner seul, mais il n'y aurait aucun endroit pour visualiser les informations qu'il rapporte sans Elasticsearch et Kibana. Bien qu'il puisse fonctionner via une API, il est plus pratique de l'exploiter de manière visuelle via Kibana sur Internet.

Pour cela, un modèle est configuré pour utiliser Elasticsearch, et un plugin est installé dans Kibana pour permettre la visualisation et la gestion de Wazuh.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/8e30f9a3-584d-4e77-b477-6b19dbe5c1f9)

Illustration 9 - Écran de connexion Wazuh-Kibana

Comme le montre l'image ci-dessus, l'écran de connexion de Kibana est personnalisé par Wazuh. Une fois que vous avez saisi le nom d'utilisateur et le mot de passe créés pendant le processus d'installation (dans ce cas, dans Elasticsearch), vous avez accès aux fonctionnalités de visualisation des informations stockées dans ce logiciel et pouvez interagir avec le serveur Wazuh via l'API, en utilisant un autre nom d'utilisateur qui a été créé lors du processus d'installation de Wazuh.

L'apparence des écrans peut varier en fonction des versions des produits utilisées.

Lorsque vous vous connectez à Wazuh-Kibana, nous vérifions la connectivité avec Elasticsearch, la disponibilité des index nécessaires, ainsi que la connexion avec le serveur Wazuh-API.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/c489c365-e4ab-418f-af93-2f65f98ebbb6)

Illustration 10 - Écran des modules Wazuh

L'écran ci-dessus affiche la page d'accueil une fois connecté avec un utilisateur valide. Les modules Wazuh sont affichés, bien qu'ils fournissent des données de fonctionnement, ces informations ne sont pas spécifiques aux conteneurs où se trouvent les différents composants. Les informations deviennent utiles lorsqu'il y a des agents qui rapportent des informations.

Par défaut, tous les modules ne sont pas activés. Par exemple, si le module de vulnérabilité n'est pas activé, ce comportement peut être modifié en modifiant la configuration de l'agent pour activer ou désactiver les modules nécessaires.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/8cf30f2a-b36e-4da2-8b55-57999bf5d770)

Illustration 11 - Écran des agents Wazuh

Dans notre cas, comme vous pouvez le voir, nous avons 4 agents disponibles et connectés, dont 2 sur des ordinateurs Linux et 2 sur des ordinateurs Windows. La version du système d'exploitation et son état sont indiqués, entre autres informations. Par exemple, l'une des fonctions des agents est d'obtenir l'inventaire des logiciels sur les ordinateurs gérés.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/49ad1a82-c735-4c57-94c8-4ecf996bb073)

Illustration 12 - Écran des événements de sécurité Wazuh

Dans la section "Security Events", nous pouvons voir que l'agent qui signale le plus d'informations sur les événements est le serveur Windows, qui est un contrôleur de domaine. Cela ne signifie évidemment pas que nous avons un problème, mais simplement qu'il y a plus d'activité en termes d'événements de sécurité.

Il incombe à l'analyste de sécurité d'effectuer des recherches et de définir des alertes sur les comportements qui pourraient constituer une menace ou même provoquer un incident.

Kibana permet de visualiser des informations à l'aide de tableaux de bord ou d'effectuer des recherches sur les données contenues dans Elasticsearch. Wazuh et ses modules ont déjà créé des tableaux de bord, mais il existe également des options pour créer tous les tableaux de bord nécessaires. Les tableaux de bord sont très utiles car ils permettent de mieux comprendre les informations de manière visuelle.

### TheHive

Pour TheHive, notre outil de gestion des incidents, la première fois que nous y accédons, une option s'affiche pour mettre à jour la base de données du produit (Cassandra dans notre cas) afin de créer les structures et les données nécessaires pour commencer à travailler avec l'outil.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/08c1516c-4d66-4337-8618-889348e434ef)

Illustration 13 - Écran de connexion à TheHive

Une fois authentifiés dans l'outil, nous avons une organisation par défaut, qui est l'organisation d'administration. Nous pouvons l'utiliser, mais il est recommandé de changer le mot de passe par défaut pour un mot de passe plus sécurisé.

La prochaine étape consiste à créer une organisation, dans notre cas, la société fictive à laquelle nous offrons nos services SOC ou CSIRT. Dans notre exemple, la société s'appelle MyHome Inc. Il suffit de donner un nom et une description à l'organisation.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/7d43fe34-a923-40f3-9b03-fda2709b93e1)

Illustration 14 - Écran de création d'organisation dans TheHive

Une fois l'organisation créée, nous pouvons déjà créer des utilisateurs au sein de cette organisation.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/283f6592-801d-4bb7-b3e8-2ddf70bf24af)

Illustration 15 - Écran des organisations dans TheHive

Pour ce faire, nous pouvons suivre le lien du nom de notre nouvelle organisation, où nous aurons la possibilité de le faire.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/b807a81e-0c24-41cc-bd38-030449126785)

Illustration 16 - Écran des utilisateurs par organisation dans TheHive

Dans notre cas, nous avons créé un utilisateur pour gérer l'organisation. Nous utilisons des adresses e-mail comme noms d'utilisateur. Nous avons créé un premier compte avec le rôle "orgAdmin" et deux autres utilisateurs pour "cortex" et "elastalert" que nous utiliserons ultérieurement. Tous les utilisateurs ont activé une clé d'API. Seul l'utilisateur administrateur de l'organisation a reçu un mot de passe, car nous en aurons besoin pour accéder à TheHive dans cette organisation.

J'ai créé les utilisateurs "cortex" et "elastalert" au sein de l'organisation, mais il serait logique de les créer dans l'organisation de gestion, à moins que nous ayons des instances différentes pour différentes organisations. Cela dépend des configurations spécifiques que vous souhaitez mettre en place.

Si ce n'était pas un environnement simulé, nous devrions créer des utilisateurs pour tous nos analystes qui contribueraient aux cas, et des profils en lecture seule pourraient également être utilisés pour les auditeurs, les directeurs, etc., des profils qui ne participeront pas aux cas mais qui pourraient nécessiter des rapports.

Une fois que nous avons créé cet utilisateur, nous nous déconnectons de l'utilisateur administrateur par défaut avec lequel nous nous sommes authentifiés dans l'outil en cliquant sur "Déconnexion" en haut à droite, au-dessus du nom de l'utilisateur.

Avant cela, nous pouvons également accéder à l'option de configuration de l'interface utilisateur, où nous pouvons modifier le format de date (par défaut, il est au format MM/JJ/AA, mais nous pouvons le changer en JJ/MM/AA, qui est plus couramment utilisé dans notre pays, mais cela dépend des préférences personnelles). Nous pouvons également configurer l'affichage des cas une fois que nous nous connectons avec notre utilisateur d'organisation. J'ai laissé cette option par défaut.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/6fcd7eec-6394-4b38-947f-6e29cb326ebe)

Illustration 17 - Écran des cas dans TheHive

Une fois authentifié en tant qu'utilisateur administrateur de notre nouvelle organisation, nous arrivons sur un écran affichant les cas. Comme nous n'en avons pas encore, nous pouvons appliquer des filtres (utiles lorsque nous avons un grand nombre de cas).

En haut à gauche, nous avons des options que nous utiliserons fréquemment pour travailler avec l'outil :

Accéder à la page d'accueil avec l'icône TheHive.
Créer de nouveaux cas.
Afficher les tâches qui nous sont affectées.
Afficher les tâches en attente.
Accéder aux alertes.
Accéder aux tableaux de bord (nous pouvons les créer ou les importer, ils peuvent être privés ou partagés).
Certaines de ces options seront utilisées plus tard dans notre cas d'utilisation.

En haut à droite, nous avons un menu "Organisation" à côté de notre nom d'utilisateur authentifié dans l'application. À partir de ce menu, nous pouvons accéder à la configuration des utilisateurs, des modèles, des tags personnalisés et de l'interface utilisateur.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/2d4b4b32-45ab-4a8f-9b96-530dce14d2de)

Illustration 18 - Écran des modèles de cas dans TheHive

Nous avons déjà vu la partie des utilisateurs précédemment, donc nous n'y reviendrons pas, mais vous pourriez être intéressé par les options de "Modèles", où nous pouvons créer des modèles avec des champs personnalisés pour cette organisation ou importer des modèles déjà créés par des tiers (par exemple, par la communauté).

Enfin, un détail important en lien avec notre prochaine section, nous avons configuré TheHive pour se connecter à Cortex. Nous pourrions également le connecter à MISP, mais pour cette démonstration de fonctionnalité, cela n'a pas été jugé nécessaire.

Pour configurer l'accès de TheHive à Cortex ou MISP, nous devons ajouter l'URL et la clé d'API correspondantes au fichier de configuration "application.conf", en ajoutant les sections appropriées, comme indiqué dans la documentation de TheHive.

Pour Cortex, notre image Docker vous permet de configurer la connexion à Cortex à l'aide de paramètres. C'est l'option que nous avons suivie, comme vous pouvez le voir sur l'écran suivant. En bas à droite, nous avons une icône qui indique en vert que nous avons une connectivité avec Cortex (en rouge en cas de problème). Nous pouvons également vérifier cela en cliquant sur l'utilisateur avec lequel nous sommes authentifiés dans l'application, puis en sélectionnant "À propos". En plus de nous donner les versions des logiciels, nous pouvons voir si la connexion avec Cortex est établie (indiqué par OK).

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/f543ecda-f0e0-48fa-b5dc-33df35619378)

Illustration 19 - Écran "À propos" et connexion entre TheHive et Cortex

### Cortex

Une fois que la base de données est initialisée, comme dans TheHive, nous pouvons accéder à notre instance de Cortex. Nous visitons l'URL où l'écran de connexion devrait apparaître.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/b447b797-9a5c-4399-b519-1d6c007de4e5)

Illustration 20 - Écran de connexion à Cortex

Comme nous l'avons fait avec TheHive, la première étape consiste à créer une organisation. Dans notre cas, nous revenons à la société fictive MyHome Inc.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/1b92efb0-dba4-467e-b5ea-4c8b1b97678c)

Illustration 21 - Écran des organisations dans Cortex

Maintenant que nous avons créé l'organisation, si nous cliquons dessus et suivons le lien, les utilisateurs nous apparaîtront. Par défaut, il n'y en a aucun, mais nous allons créer deux utilisateurs : un utilisateur avec tous les rôles, qui sera notre administrateur (nommé "Paola"), et un utilisateur avec les rôles "read" et "analyze", qui sera utilisé par TheHive pour interagir avec Cortex. Cet utilisateur n'a pas besoin de mot de passe, seule la clé d'API est nécessaire, comme nous l'avons mentionné précédemment lors de la configuration dans le fichier "application.conf".

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/34ac905f-bf99-4c24-a61a-6a655c1a6d02)

Illustration 22 - Écran des utilisateurs par organisation dans Cortex

Maintenant, nous nous déconnectons et nous nous connectons à l'application avec notre nouvel utilisateur administrateur d'organisation, "murdock". Nous devons maintenant configurer Cortex.

Dans Cortex, nous avons deux types de jobs : les "analyzers" (analyseurs) et les "responders" (répondeurs). Nous commencerons par configurer les analyseurs. Dans les deux cas, il existe des services gratuits et payants qui nécessitent une clé d'API, laquelle est généralement nécessaire pour configurer et activer les analyseurs et certains répondeurs.

Dans mon cas, j'ai quelques clés d'API (comme SHODAN) que j'ai en mode "Trial" ou "Free Use". Ces clés d'API ont des limitations d'utilisation, mais puisqu'il s'agit d'un environnement simulé, le volume de requêtes ne sera pas élevé, donc j'espère que cela sera suffisant.

Avec plus de sources de renseignements et de meilleure qualité, nous pourrons obtenir des informations fiables et utiles pour les observateurs de nos cas dans TheHive.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/af8e651d-b3f8-4214-9c1d-3510c5d761ff)

Illustration 23 - Écran des paramètres des analyseurs dans Cortex

Si nous accédons à la section "Organisation", nous pouvons voir les utilisateurs (que nous avons déjà vus auparavant), puis nous allons dans "Analyzers Config", où nous pouvons configurer les analyseurs. Comme vous pouvez le voir sur l'image, il y en a jusqu'à 81 disponibles (ces paramètres sont communs à plusieurs analyseurs).

Dans notre cas, nous activerons certains analyseurs. Pour cela, il suffit de cliquer sur "Edit" et de remplir les informations demandées dans le formulaire. Sur cet écran, vous pouvez voir quelles informations sont nécessaires pour chaque analyseur (les cases cochées représentent les informations fournies).

Parmi les analyseurs, nous en avons un que nous examinerons plus en détail dans la section suivante : MISP. C'est là que notre composant de plateforme SIRP entre en jeu.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/f736300e-3c3f-413a-8c1e-8e0e8fc9b1ff)

Illustration 24 - Écran de configuration de l'analyseur MISP dans Cortex

Une fois les modules ou plugins que nous allons utiliser configurés, nous passons à la section suivante : "Analyzers". Il y en a 164 disponibles (qui utilisent les 81 configurations de paramètres mentionnées précédemment).

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/e84a4876-6668-46b2-9fc3-196ddbd64daf)

Illustration 25 - Écran d'activation/désactivation des analyseurs dans Cortex

Pour les activer, il suffit de cliquer sur "Enable" et un formulaire s'affiche pour demander des informations de configuration, telles que le temps de mise en cache, le délai pour les requêtes, etc. J'ai laissé les options par défaut en attendant de les tester et de voir si des ajustements sont nécessaires.

Ensuite, nous pouvons voir les paramètres pour notre analyseur MISP. Comme vous pouvez le voir sur l'image, certaines informations sont incomplètes en haut, mais les paramètres que nous avons déjà configurés apparaissent ainsi que les nouveaux paramètres mentionnés précédemment.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/4d3cc30d-f68d-4393-980a-b6bd27431200)

Illustration 26 - Écran d'activation de l'analyseur MISP dans Cortex

Au moment de la rédaction de ce document, j'ai activé les modules de réponse suivants (en attendant de confirmer certaines demandes d'accès à d'autres services) :

AbuseIPDB_1_0
Shodan_DNSResolve_1_0
Shodan_Host_1_0
Shodan_Host_History_1_0
Shodan_InfoDomain_1_0
Shodan_ReverseDNS_1_0
Shodan_Search_2_0
SpamhausDBL_1_0
TalosReputation_1_0
TeamCymruMHR_1_0
Threatcrowd_1_0
TorBlutmagie_1_0
TorProject_1_0
URLhaus_2_0
UnshortenLink_1_2
Urlscan_io_Scan_0_1_0
Urlscan_io_Search_0_1_1
VirusTotal_GetReport_3_0
VirusTotal_Scan_3_0
Virusshare_2_0
Chaque analyseur est spécialisé dans des domaines différents, tels que la réputation des adresses IP, les noms DNS, les recherches d'observables dans les emails, le réseau TOR, les URL, l'analyse de fichiers suspects, etc.

Généralement, les analyseurs sont des programmes écrits en Python. Certains d'entre eux s'exécutent sous forme de conteneurs Docker, ce qui est courant pour les analyseurs et les répondeurs.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/e8cbc765-8bba-4229-8523-55ebac57d77a)

Illustration 27 - Écran de configuration des répondeurs dans Cortex

De même, nous avons une section de configuration des paramètres qui nous permet d'activer ou de désactiver les répondeurs. La logique est similaire à celle des analyseurs.

En ce qui concerne les répondeurs, le choix est moins étendu (actuellement 22). Cela dépend de l'infrastructure dont dispose l'entreprise ou l'organisation à défendre. Il s'agit donc d'un point d'amélioration pour Cortex en termes de prise en charge des produits (pare-feu, IPS, etc.). C'est un défi pour la communauté et les utilisateurs de ce produit, qui peuvent également contribuer à son développement.

Dans notre cas, nous avons activé les répondeurs suivants :

Virustotal_Downloader_0_1
Wazuh_1_0
Le premier est bien connu dans le domaine de la sécurité, tandis que le second correspond à notre outil Wazuh. Comme expliqué précédemment, Wazuh agit en tant que répondeur dans ce cas.

Un utilisateur Wazuh a donc été créé pour ce type de réponse. Nous nous rendons dans Wazuh Kibana et créons l'utilisateur "cortex".

Dans ce cas, étant donné qu'il s'agit d'un environnement simulé, nous avons configuré l'utilisateur avec le rôle "admin". Cependant, dans un environnement réel, il serait préférable de limiter les autorisations au strict minimum nécessaire. Dans ce cas, j'ai choisi cette configuration pour m'assurer que cela fonctionne plutôt que de passer du temps à tester différentes autorisations pour vérifier si elles sont suffisantes.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/73886a2c-64c7-48d5-bd36-fc58a3369511)

Illustration 28 - Écran des utilisateurs Wazuh

Pour conclure, nous avons toujours considéré Cortex comme un allié de TheHive, mais il peut également être utilisé individuellement. En haut à gauche, nous pouvons voir l'option "New Analysis", qui nous permet de soumettre différents types de données (observables) à analyser par Cortex.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/59b3529e-25f5-4d7b-98e0-f7f8aa4be940)

Illustration 29 - Écran de lancement de l'analyse dans Cortex

### MISP

Une fois le produit installé et opérationnel, nous avons une base de données vide, tout comme dans les cas précédents. Pour accéder au service, nous utilisons la console Web en suivant l'URL, où l'écran de connexion de MISP apparaît.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/1a94e9b0-efbe-499b-9b3d-d79ac6828f75)

Illustration 30 - Écran de connexion à MISP

Nous nous connectons en tant qu'administrateur par défaut (admin@admin.test) et procédons à la création de notre organisation fictive, qui représente la société cliente pour laquelle notre plateforme SIRP fournit des services.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/46db289d-922e-4943-8d82-057b64d6e06b)

Illustration 31 - Vue des organisations dans MISP

Sur cet écran, nous voyons notre organisation par défaut et celle que nous avons créée, MyHome Inc. Pour créer une nouvelle organisation, nous accédons au menu "Administration" et sélectionnons "Add Organization", ou nous cliquons simplement sur "Administration" et trouvons l'option "Add Organization" dans le menu latéral gauche.

Nous remplissons quelques champs, tels que le nom, un identifiant unique (qui peut être généré automatiquement), une description, la nationalité, le secteur, etc.

MISP offre de nombreuses options de configuration et d'actions sur le produit, mais dans ce TFM, nous n'en examinerons que quelques-unes.

Ensuite, comme pour les autres composants, nous créons un utilisateur pour la nouvelle organisation et un utilisateur pour notre Cortex, afin de pouvoir travailler avec notre plateforme MISP via l'API.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/33fcf051-b8fa-412e-afb7-a3edaff92238)

Illustration 32 - Écran des utilisateurs dans MISP

Comme pour les cas précédents, une adresse e-mail est requise. Nous avons donc créé un utilisateur avec des autorisations en lecture seule, ce qui devrait suffire si la fonction de Cortex est prise en compte.

Nous nous déconnectons et nous connectons à l'application MISP avec notre nouvel utilisateur.

À ce stade, notre base de données est toujours vide. Nous devons donc lui donner du contenu. Pour cela, nous examinons notre liste de "Feeds".

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/780932d4-181e-44c0-b54f-6faeafbe23d2)

Illustration 33 - Écran de la liste des flux dans MISP

Par défaut, deux flux sont fournis, du moins dans mon installation utilisant le référentiel MISP sur GitHub. Pour les afficher et les activer (ils sont désactivés par défaut), nous accédons à "Sync Actions" et "List Feeds". Dans les actions à droite, nous trouvons l'option pour les activer.

L'une des fonctionnalités de MISP est de permettre le partage d'informations conformément à une norme de partage. Sur la page MISP Default Feeds (misp-project.org) du projet MISP, nous pouvons trouver une liste de flux que nous pouvons ajouter à notre installation.

En dehors de cette liste de flux, il serait judicieux de se connecter et d'échanger des informations avec d'autres instances MISP de notre environnement, qu'il s'agisse d'organisations de notre secteur ou d'organismes officiels de CERT.

Ces flux peuvent être téléchargés à tout moment, comme le montrent les actions à côté de chaque flux dans la liste des événements. Cependant, vous devez également activer régulièrement ces mises à jour pour rester à jour avec les informations qui circulent dans le monde.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/123c212d-4cef-4c92-9ae5-770b73f6719b)

Illustration 34 - Écran de planification des tâches dans MISP

Dans le menu "Administration", nous avons des "Scheduled Tasks" pour programmer la mise à jour de nos flux (ceux de la liste ci-dessus) et du cache maintenu dans MISP.

Vous pouvez programmer la fréquence d'exécution et l'heure de la première exécution. Le champ est modifiable en cliquant dessus. La colonne "Next Run" indique la prochaine exécution, bien que, à mon avis, elle ne montre que la date, ce qui rend difficile de connaître l'heure exacte sans calculer à partir de l'heure de début et de la fréquence configurée.

Dans notre cas, nous voyons qu'une tâche sur deux est terminée. Nous pouvons vérifier comment cela s'est déroulé en accédant à l'option "Jobs".

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/9af4ad7d-1169-4dd1-9641-f6980ef962e8)

Illustration 35 - Écran des tâches dans MISP

Apparemment, l'exécution du flux 1 semble fonctionner correctement, mais pour le flux 2 (numéro attribué dans la liste des flux), le téléchargement échoue toujours (cela doit être vérifié pour déterminer s'il y a un problème avec l'URL ou l'accès, etc.). Cependant, le téléchargement manuel, qui est la première ligne de l'écran précédent, semble avoir fonctionné sans erreur, du moins en apparence.

En parlant d'erreurs, il est également intéressant de mentionner l'option "Server Settings and Maintenance" du même menu "Administration".

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/9eb44a94-386a-4f1e-9c89-d97e7eba8fb1)

Illustration 36 - Écran des paramètres du serveur et de la maintenance dans MISP

C'est une sorte d'option d'auto-diagnostic du logiciel MISP. Elle permet également de modifier certains paramètres à partir de ces écrans. Idéalement, tout devrait être en vert, mais j'ai passé un peu de temps à résoudre certains "Critical Settings", la plupart étant dus à des paramètres non configurés qui sont laissés par défaut (par exemple, la langue).

Enfin, pour conclure cette section, il est important de noter que MISP permet également de partager des informations avec notre environnement. Il est possible d'ajouter nos propres événements avec des informations IOC spécifiques à notre organisation. Nous pouvons utiliser les balises déjà créées pour les événements importés et créer nos propres balises pour mieux organiser les données que nous ajoutons depuis notre organisation.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/c4310b5a-f8da-4131-8647-0099ce0343a2)

Illustration 37 - Écran de la liste des événements dans MISP

Pour cela, nous pouvons utiliser le menu "Event Actions", où l'option "List Tags" nous permet également d'ajouter des balises. Dans notre cas, nous avons ajouté une balise "Manual" lorsqu'un analyste ajoute des informations à notre MISP manuellement.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/93c1db83-8dad-4850-86b8-78ae6750dcc1)

Illustration 38 - Écran de la liste des balises dans MISP

### Praeco-Elastalert

Voici deux composants, Praeco et Elastalert, qui sont intégrés entre eux de la même manière que l'intégration précédente entre Wazuh, Elasticsearch et Kibana, avec quelques différences.

Dans cette configuration, Elastalert agit en tant que backend, tandis que Praeco est un frontend web. Elastalert fonctionne au niveau des fichiers pour générer des règles et effectuer des recherches dans Elasticsearch. Il enregistre également ses métadonnées et son audit dans Elasticsearch, ce qui lui permet de générer ses propres index. Les informations d'accès à Elasticsearch, telles que l'utilisateur/mot de passe ou le jeton utilisateur, peuvent être spécifiées dans le fichier de configuration lors de l'installation.

En ce qui concerne la sortie des alertes, Elastalert offre plusieurs options, telles que JIRA, Slack, l'e-mail, et surtout, TheHive, qui nous intéresse particulièrement pour transmettre les informations aux analystes de sécurité de l'organisation.

Praeco est l'interface web qui facilite la configuration des alertes à l'aide de formulaires et affiche les métadonnées d'Elastalert.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/6c71de9b-49ee-44f7-b4da-d71a5e269991)

Illustration 39 - Écran principal de Praeco

Cependant, un autre défaut du produit est son manque d'authentification, ce qui signifie qu'il permet l'accès à toute personne ayant la visibilité de l'URL. Cela peut être dangereux car cela permet à des utilisateurs non autorisés d'effectuer des requêtes sur Elasticsearch, qui contient des informations sensibles sur l'organisation. Pour remédier à ce problème, nous recommandons fortement de configurer NGINX pour fournir un cryptage TLS et d'ajouter une authentification de base.

Lorsque vous accédez à la console Web de Praeco, vous remarquerez un menu sur la gauche de l'écran. Les dossiers «Rules» et «Templates» sont vides après l'installation, à moins que vous n'importiez du contenu provenant de la communauté. Cela signifie qu'aucune règle n'est définie par défaut.

Dans le cadre de ce test, nous avons créé une règle appelée «SSH Failed Login». Cette règle interroge Elasticsearch pour rechercher plus de deux connexions SSH ayant échoué (ce nombre est trop faible pour générer une alerte réelle). Si la règle détecte un dépassement du seuil, une alerte est générée dans TheHive. Cette alerte peut ensuite être traitée en l'ajoutant à un cas ou en suivant le processus approprié que vous jugez le plus pratique.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/ea4a3251-5f53-44ca-b001-7f294d3d38ce)

Illustration 40 - Écran d'Alertes de TheHive (alertes créées par Elastalert)

Pour générer des alertes pertinentes, il est important de jouer avec les compteurs d'événements, la fréquence, et d'autres paramètres. Ces paramètres déterminent si une alerte est utile en évitant autant que possible les faux positifs, tout en détectant les comportements qui représentent de réelles menaces et/ou des incidents potentiels.

En revenant à Praeco, vous pouvez voir ci-dessous l'écran où les règles sont définies pour déterminer quand une alerte doit être générée. Vous spécifiez l'index à surveiller et utilisez un langage similaire à SQL pour générer des requêtes, déterminer les champs à afficher, le titre de l'alerte, etc.

Pour chaque règle, ces données peuvent être différenciées, y compris la police utilisée. Vous pouvez même utiliser différentes sources Elasticsearch si nécessaire, bien que cela nécessiterait une configuration manuelle, car Praeco ne prend pas en charge cette fonctionnalité directement.

Dans notre cas, la configuration de TheHive est également stockée dans un fichier situé dans le répertoire «Rules», qui a un rôle spécial. Ce fichier permet d'enregistrer l'URL et les informations d'accès, et peut ensuite être utilisé par toutes les règles. Il utilise la clé d'API de l'utilisateur «elastalert», comme nous l'avons vu dans la section de configuration de TheHive.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/8675b273-005a-4844-bac1-6b187af456ba)

Illustration 41 - Écran de définition de règle dans Praeco

En plus des étapes précédentes, il est essentiel de créer un utilisateur dans Elasticsearch et de lui attribuer les autorisations nécessaires pour créer et mapper les index requis pour utiliser Elastalert, ainsi que pour consulter les données sur lesquelles les alertes sont basées.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/9c102a9c-2a10-4aae-a93d-198aa3574800)

Illustration 42 - Écran des utilisateurs Elasticsearch (utilisateur "elastalert")

Dans la section "Internal Users" de Wazuh-Kibana, vous pouvez créer un autre utilisateur nommé "elastalert" de la même manière que vous l'avez fait pour TheHive. Vous devez attribuer un nom d'utilisateur et un mot de passe à cet utilisateur.

Cela peut être fait via l'interface web de Wazuh-Kibana, où vous avez également la possibilité de créer un rôle pour attribuer les autorisations nécessaires à ce nouvel utilisateur.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/388bae17-b075-4c5d-92d2-49af5b57c883)

Illustration 43 - Écran des rôles Elasticsearch (autorisations "elastalert")

Pour gérer les autorisations de l'utilisateur "elastalert", vous pouvez configurer des rôles dans Elasticsearch. Les rôles permettent de définir les autorisations spécifiques pour cet utilisateur. Vous pouvez attribuer des autorisations pour créer et mapper les index nécessaires à Elastalert, ainsi que pour consulter les données nécessaires pour générer les alertes.

En résumé, grâce à toutes les configurations réalisées lors de votre travail de fin de Master (TFM), vous avez créé votre propre plate-forme SIRP (Security Incident Response Platform).

### Cas d'utilisation

#### Description
Dans le cadre de notre plate-forme SIRP, nous allons décrire un cas d'utilisation qui met en œuvre tous les composants de notre environnement de simulation.

1. Événements de sécurité détectés par Wazuh : Nous utilisons les agents Wazuh et le serveur Wazuh pour détecter des événements de sécurité en fonction de leurs règles. Ces événements sont stockés dans notre plateforme Elasticsearch, qui fait partie de la solution Wazuh.

2. Création d'une alerte intégrée à TheHive : Nous créons une alerte spécifique pour détecter l'événement ou le groupe d'événements choisi. Cette alerte est ensuite intégrée à TheHive, conformément aux paramètres présentés dans la section 4.5.

3. Traitement de l'alerte dans TheHive : À ce stade, TheHive entre en jeu pour prendre en charge l'alerte et la traiter avec notre outil de gestion des incidents. Nous pouvons créer un cas, ajouter des observables pertinents et effectuer d'autres actions nécessaires.

4. Analyse des observables avec Cortex : Une fois que nous avons des observables dans TheHive, nous pouvons les analyser en utilisant les analyseurs disponibles dans Cortex. Cela nous aide à déterminer si les observables sont problématiques, s'ils sont des faux positifs, etc.

5. Enrichissement des cas avec MISP : Parmi les analyseurs disponibles, nous avons notre plateforme MISP, qui est l'une des sources d'informations permettant d'enrichir le cas en fonction des résultats obtenus.

6. Réponse et clôture du cas : En fonction des découvertes et des analyses effectuées, nous pouvons décider de la nature de la réponse à apporter à l'aide des modules disponibles dans Cortex. Il peut s'agir de prendre des mesures supplémentaires pour répondre à la menace ou simplement de finaliser la documentation du cas et de le fermer.

Ce cas d'utilisation met en œuvre tous les composants de notre solution SIRP et illustre un scénario réaliste auquel un analyste de sécurité pourrait être confronté dans n'importe quelle entreprise ou organisation.

#### Tests effectués

Avant de procéder à l'utilisation réelle, des tests unitaires ont été effectués sur chaque composant pour valider leur bon fonctionnement et s'assurer que le cas d'utilisation se déroulerait correctement lors de son exécution.

Un scénario de simulation a été élaboré conformément aux étapes décrites ci-dessous, impliquant l'utilisation de tous les composants de l'environnement qui ont été mis en place dans le cadre de ce TFM.

Le scénario en question simule une possible attaque par force brute par un "insider" (un membre du personnel interne de l'entreprise).

##### Étapes: 

1. L'objectif est de simuler une attaque par force brute en effectuant un grand nombre de tentatives de connexion échouées à l'aide d'un script exécuté depuis la machine cliente Windows (WINCLT001) contre le compte utilisateur root sur la machine PRX001.

2. Cette action génère des événements de sécurité, qui sont ensuite détectés et enregistrés dans notre console Web Wazuh-Kibana. De plus, cette attaque est également identifiée comme un possible TTP (Tactic, Technique, Procedure) de l'attaque par force brute, selon la classification de Mitre.

3. Une alerte spécifique est créée dans Praeco-Elastalert pour détecter ces tentatives de connexion échouées sur une courte période. Les critères d'alerte sont configurés pour détecter plus de 10 événements par minute, ce qui est clairement anormal et indique une attaque par force brute automatisée.

4. À la suite de ces alertes, plusieurs alertes sont générées dans TheHive, car la définition d'alerte spécifie qu'en cas de répétition du comportement pendant un certain laps de temps, une nouvelle alerte doit être générée.

5. Dans TheHive, en tant qu'administrateur et analyste (étant donné notre effectif réduit), nous examinons les alertes et constatons qu'il y en a plusieurs provenant de la même source, car nous avons configuré l'alerte pour inclure la ligne de "full_log" qui contient les informations pertinentes. Étant donné que l'alerte dans Wazuh ne fournit pas directement l'adresse IP source en tant que champ distinct, nous devons extraire cette information de la ligne complète du journal.

6. Nous ouvrons un cas dans TheHive en sélectionnant l'une des alertes. Comme nous n'avions aucun cas existant, il apparaît comme "Empty". Si nous cliquons sur le bouton "Yes, import", un nouveau cas est créé avec un numéro d'identification et le titre de l'alerte en tant que nom du cas. Il serait également possible d'ouvrir un cas directement avec un titre personnalisé et ensuite fusionner l'alerte (ou les alertes) dans ce cas.

7. Nous ajoutons un nouvel observable au cas et incluons l'adresse IP source provenant de l'alerte. Nous ajoutons également un tag et une description appropriés pour cet observable.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/19fac6a3-4581-4df8-a992-ad8167db2b00)

8. Bien que nous sachions qu'il s'agit d'une adresse IP interne, nous essayons néanmoins de soumettre cette adresse IP à nos "analyseurs" disponibles dans Cortex. En cliquant sur l'adresse IP de l'observable, une liste des "analyseurs" Cortex disponibles est affichée.

9. Nous pouvons exécuter tous les "analyseurs" en utilisant l'option "Run All", bien que nous sachions que nous n'obtiendrons pas de résultats significatifs. Pour notre test, nous avons utilisé les analyseurs MISP, Shodan et AbuseIPDB, qui sont configurés pour prendre en charge les adresses IP.

10. Comme prévu, les résultats des "analyseurs" indiquent que l'adresse IP n'a pas pu être trouvée ou ne contient que peu d'informations pertinentes. Cela est dû au fait qu'il s'agit d'une adresse IP privée de notre réseau d'entreprise simulé.

11. Bien que nous n'ayons pas trouvé de preuve d'activité malveillante, il est important de souligner que cette activité est illégale, car nous savons qu'il est impossible de générer autant de connexions par minute légitimement.

12. Dans ce cas, nous décidons d'utiliser les fonctionnalités de réponse de notre Cortex avec Wazuh. Wazuh est configuré pour fournir des capacités de réponse aux incidents, comme indiqué dans la section 4.3 lors de la configuration de Cortex.

13. Dans l'onglet "Observables", une icône en forme de roue dentée apparaît à droite, sous la section "Actions". Nous cliquons sur cette icône et sélectionnons "Wazuh" comme option de réponse.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/b4b13167-8940-45e5-a7d5-fc255fe2bed8)

14. Une fenêtre de confirmation apparaît, et nous cliquons sur "Yes, run it" pour confirmer l'exécution de l'action de réponse avec Wazuh.

15. À partir de ce moment, il ne devrait plus être possible d'accéder à cette machine à partir de l'appareil concerné. Cela constitue une mesure temporaire en attendant de mener d'autres investigations que nous ne pouvons pas réaliser directement avec les outils disponibles (par exemple, vérifier l'ordinateur de l'utilisateur). Nous pouvons maintenant identifier qui était le propriétaire de l'adresse IP, grâce aux informations disponibles dans Wazuh, etc.

16. Dans notre cas, nous ajoutons un autre observable de type "hostname" pour indiquer à quelle machine l'adresse IP est associée.

17. À partir de ce stade, la décision de poursuivre ou de clore l'affaire dépend de la politique de l'entreprise ou de l'organisation. Il est possible de laisser l'affaire ouverte en attendant d'obtenir des informations supplémentaires (par exemple, des informations provenant de l'examen de l'ordinateur de l'utilisateur) ou de la clôturer.

18. Pour notre exemple de cas d'utilisation, nous procédons à la clôture de l'affaire et avons la possibilité de rédiger un résumé de l'incident pour archiver toutes les informations pertinentes et les actions prises dans le cadre de la réponse à cet incident.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/4569c8a7-3f6f-4e7c-adf5-61187009a2cc)

### Conclusions

Les travaux réalisés ont permis d'atteindre l'objectif de disposer d'une plateforme de gestion des incidents entièrement opérationnelle, avec des capacités de détection, d'analyse et de réaction.

Cependant, il reste des améliorations à apporter à l'installation et à la configuration des produits. Voici quelques-unes des améliorations suggérées pour votre environnement :

* Homogénéiser la configuration des volumes, des secrets et des certificats dans les différents composants de la plateforme.

* Utiliser le service d'équilibrage intégré dans les nœuds et permettre la visibilité des microservices vers l'extérieur via les composants Ingress.

* Intégrer un DNS externe avec kubeDNS (DNS interne de Kubernetes) pour une meilleure résolution des noms de domaine.

* Améliorer la sécurité en utilisant des stratégies réseau au sein du cluster Kubernetes pour limiter l'accès aux composants.

* Limiter les ressources que les composants peuvent utiliser afin d'optimiser les performances et de garantir une utilisation efficace des ressources disponibles.

* Ajouter une surveillance pour vos propres clusters Kubernetes afin de détecter et redémarrer automatiquement les pods en cas de dysfonctionnement.

* Renforcer la sécurité des composants, notamment en améliorant la gestion des certificats et en désactivant les certificats auto-générés non vérifiés.

* Modifier les paramètres par défaut des composants pour renforcer la sécurité de la solution.

* Implémenter l'authentification de base via NGINX pour les composants qui n'ont pas de système d'authentification intégré, comme Praeco.

* Mettre en place une gestion des utilisateurs via une source externe telle qu'un annuaire Active Directory (AD) ou un service LDAP, et envisager une gestion intégrée avec des composants de Single Sign-On (SSO) pour une expérience d'authentification améliorée.

* Utiliser Logstash et les agents Beat pour collecter les journaux d'autres produits que le système d'exploitation, afin d'améliorer la visibilité et la collecte de données de sécurité.

* Optimiser la configuration d'Elasticsearch pour en faire un véritable SIEM, en optimisant l'utilisation des index, des shards, des réplicas, etc.

* Utiliser Elasticsearch pour stocker les données de TheHive et de Cortex, en les intégrant à votre installation existante.

* Améliorer les capacités d'analyse et de réaction de Cortex en intégrant des sources de renseignement externes payantes.

* Favoriser le partage d'informations avec d'autres organisations, entités et organismes officiels via la composante MISP, afin de renforcer la collaboration et de bénéficier d'une plus grande visibilité sur les menaces.

* En mettant en œuvre ces améliorations, vous pourrez renforcer la sécurité, l'efficacité et les fonctionnalités de votre plateforme SIRP, en la rendant plus adaptée aux besoins de votre entreprise ou de votre organisation.
