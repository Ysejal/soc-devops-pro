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
•      VirusTotal : peut être intégré pour permettre la détection de fichiers malveillants à l'aide du BBDD de VirusTotal
•      Custom Integration : utilisé pour l'intégration avec des produits tiers, il s'agit d'une option générique qui permet l'intégration par script

Il est possible d'accéder à Wazuh Server via le Web avec un navigateur ou d'interagir avec celui-ci via l'API Wazuh, une API RESTful qui permet de gérer le Wazuh, ainsi que d'effectuer diverses requêtes.
L'intégration avec Kibana, comme indiqué ci-dessus est l'une des fonctionnalités de ce produit, dispose d'un plugin qui permet aux utilisateurs de visualiser et d'analyser les alertes Wazuh stockées sur Elasticsearch. Les utilisateurs peuvent obtenir des statistiques par agent, rechercher des alertes et les filtrer en utilisant différentes vues. S'intègre à l'API Wazuh pour récupérer des informations sur la gestion et la configuration des agents, des journaux, des règles définies, des groupes, etc.
Parmi les autres fonctionnalités du produit, citons l'analyse des vulnérabilités basée sur les BDD CVE et la détection du logiciel installé, le Wazuh fait également l'inventaire, ainsi que le scan de compatibilité des stratégies et des paramètres de sécurité de l'ordinateur.

###Filebeat

Il s'agit d'un agent léger qui vous permet de transférer et de centraliser des données de journaux. Il surveille les fichiers configurés et permet de les envoyer à Elastichsearch ou Logstash pour indexation.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/87d894a9-07f7-4983-9bb9-78613adff3d6)
Illustration 4 - Surveillance et envoi des journaux

Pour collecter ces journaux, vous devez installer cet agent sur chacun des ordinateurs que vous voulez surveiller et envoyer des journaux à notre SIEM et configurer les fichiers à partir desquels vous voulez recueillir des informations.

Vous devez également indiquer le format de ces journaux, afin que vous puissiez « couper » les informations de sorte que ces fichiers puissent ensuite être exploités par le biais de requêtes.

Nous disposons de versions pour Windows et Linux, la dernière version disponible est la version 7.12 au moment de l'exécution de ce travail d'analyse. Il peut également être installé sur Docker.

###Winlogbeat

Son rôle est similaire à celui de l'agent Filebeat que nous avons vu au point précédent, bien qu'il s'agisse ici d'un composant spécifique de systèmes Windows, car il vous permet de collecter et d'envoyer les informations générées dans le journal des événements de ce S.O.
Bien qu'il puisse être utile de disposer de tous les événements Windows centralisés dans Elastic dans notre cas, nous nous concentrerons sur ceux du journal des événements de sécurité.

###Auditbeat

Cet agent est installé sur les plates-formes Linux et vous permet d'extraire des informations des librairies Audit de ce S.O. sans avoir à modifier la configuration du démon audited.

De cette façon, nous pouvons envoyer des données sur les actions réalisées par les utilisateurs sur le S.O. Linux à la plateforme SIEM.

###Packetbeat

Il s'agit de la dernière des « beats » à analyser pour notre plateforme SIRP en tant qu'élément de collecte d'informations réseau.
Cet agent vous permet de surveiller le trafic réseau et d'analyser en temps réel les paquets envoyés ou reçus par un hôte ou un conteneur et de les envoyer à Logstash ou Elasticsearch.
Cette fonctionnalité peut être très utile sur notre plate-forme si nous ne pouvons pas avoir un NIDS ou un NIPS dans tous les emplacements réseau de notre organisation et peut nous permettre d'analyser le trafic réseau entre les ordinateurs de notre réseau local, trafic que nous ne ferons généralement pas passer par un NIDS ou un NIPS.
Packetbeat peut analyser les paquets et décoder les données au niveau de l'application HTTP, DNS, etc. en donnant quelques exemples de protocoles de couche 7.

###Logstash

C'est un composant de la suite de produits Elastic qui permet la réception de journaux, le sablage et la transformation en temps réel, indépendamment de leur format ou de leur complexité. Dans notre cas, il est utilisé pour envoyer les données à Elasticsearch, mais la sortie de données peut être autre, envoi par courrier, bbdd, etc.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/604461db-e734-440b-a487-b0cb0b1dca69)
 Illustration 5 - Billets / Sorties Logstash
Installable sur Linux, Windows, MacOS ou dans Docker

###Fonctionnalités Logstash

Entrées : Entrées facilement à partir de vos journaux, métriques, applications Web, entrepôts de données.
Filtres : vous pouvez configurer des filtres personnalisés, même si le produit dispose déjà d'une bibliothèque de modèles.

•      Dérive la structure à partir de données non structurées avec grok

•      Décrypte les coordonnées géographiques à partir des adresses IP

•      Il anonymise les données PII et exclut totalement les champs sensibles

•      Facilite le traitement global, quelle que soit la source de données, le format ou le schéma.

Sorties : Elasticsearch est la base de la suite de produits pour la recherche et l'analyse de données, mais ce n'est pas la seule option possible. Logstash dispose d'une variété de sorties qui vous permettent d'acheminer les données où vous le souhaitez, ce qui vous donne la possibilité de déverrouiller un grand nombre de cas d'utilisation ultérieurs.

###Elasticsearch

C'est le noyau de la suite, le composant central de l'outil. C'est un moteur de recherche et d'analyse distribué avec une API RESTful.

L'une de ses principales fonctionnalités est de faciliter la recherche et dispose d'un indexeur qui vous permet d'organiser les informations de manière à pouvoir répondre rapidement aux requêtes que vous effectuez.

Conçue pour prendre en charge de gros volumes de données, c'est une solution évolutive que vous pouvez créer un cluster qui vous permet d'ajouter des nœuds pour une plus grande capacité de traitement des données.

L'outil est centré sur la recherche, il permet des recherches structurées et non structurées.

Actuellement, la dernière version disponible du produit est la version 7.12 et peut être installée sur Linux, Windows ou MacOS, ainsi que sur la technologie de conteneur Docker.

###Fonctionnalités d'Elasticsearch

Il permet la gestion des nœuds dans « Data tiers », il est possible de configurer les nœuds en fonction des performances des nœuds dans trois catégories Hot, Warm et Cold et il permet d’automatiser le déplacement des données entre les nœuds, bien que nous ne l’utilisions pas pour ce module de gestion du trafic sécurisé, dans un véritable SIEM, nous pourrions enregistrer les journaux les plus récents sur des nœuds de type Hot, où la recherche doit être en temps réel et où les données les plus anciennes pourraient être reléguées à des nœuds de stockage à faible coût de type Cold, par exemple, ce qui peut être nécessaire dans les organisations qui ont besoin de stocker des journaux pendant de longues périodes en raison d’exigences légales

De nombreuses fonctionnalités du produit sont disponibles pour les duplications, snapshots, etc., en tant que bon produit à utiliser dans un environnement d'entreprise sérieux, vous avez de multiples options pour maintenir les performances et la disponibilité, nous ne traiterons pas ces fonctionnalités ici car l'utilisation du produit que nous ferons dans ce TFM ne fera que gratter la surface.

Il dispose également d'un ensemble de fonctionnalités de sécurité, nécessaires pour ce type de produits qui peuvent contenir des données sensibles, l'une des choses de base est qu'un bon contrôle d'accès est nécessaire (l'outil fournit des mécanismes RBAC et ABAC pour affiner l'accès aux différentes ressources stockées dans Elastic) et enregistre un journal d'audit des accès corrects ou erronés pour pouvoir détecter les attaques. Un autre point important est le cryptage des données, si possible dans les communications avec Elasticsearch, ce qui serait le cryptage des données locales qui serait délégué au S.O. des nœuds.

Une autre fonctionnalité de sécurité disponible pour le produit est le filtrage IP, vous pouvez appliquer le filtrage IP aux clients d'application, aux clients de nœud ou clients de transport, ainsi que d'autres nœuds qui tentent de joindre le cluster Elastic.
En ce qui concerne l'authentification, il existe plusieurs méthodes pour la gérer : native, LDAP, SAML, etc. et permet d'utiliser SSO avec Kibana (il n'est pas nécessaire d'utiliser différents utilisateurs/passwords lors de l'accès via Kibana ou directement à Elasticsearch).
Elasticsearch permet de définir des alertes et dispose d'un large éventail de notifications possibles : email, webhooks, IBM Resilient, Jira, Microsoft Teams, Slack, etc.
Le produit fournit une API RESTful et JSON puissantes, ce qui permet l'accès au produit via du code d'application dans plusieurs langages Java, Go, .NET, PHP, Javascript, Perl, Python, Ruby, etc.
Vous disposez d'autres fonctionnalités que vous pouvez consulter sur le site Web du produit, mais que nous ne détaillerons pas dans ce document.

###Kibana

Ce composant est une interface utilisateur open source qui vous permet de visualiser les données Elasticsearch de manière graphique et visuelle via votre interface Web.

Les principales caractéristiques seront examinées, bien que le produit ait de multiples possibilités d'exploitation des données, des tableaux de bord, etc. Nous ne passerons pas beaucoup de temps car, comme cela a été dit, il s'agit essentiellement d'une interface de visualisation des données.

Dans notre cas, il nous permettra de visualiser les données collectées dans Elasticsearch à partir de différentes sources et en particulier de Wazuh. Ce peut être un outil très utile pour un analyste pour trouver des motifs, des tendances ou même sa capacité à montrer des informations géospatiales (sur une carte).

Comme le reste de la suite ELK est disponible pour Linux, Windows et MacOS et la plate-forme de conteneurs Docker.

Capacités de Kibana

Comme cela a déjà été dit, la principale capacité du produit est l'affichage des données.

•      Kibana Lens vous permet de créer des écrans de données avec drag-and-drop

•      Time series, charts, métriques, tables de données, informations géopositionnelles (représentant des données sur une carte), etc. sont des éléments que vous pouvez configurer et afficher dans Kibana

•      Créer et configurer des tableaux de bord

•      Interface console, vous permet d'utiliser du code pour interroger Elasticsearch

•      Générer des graphes d'analyse

•      Permet l'exportation de rapports en mode PDF ou image PNG

•      Exporter les informations via CSV

###Analyse des composants de gestion des incidents

Jusqu'à présent, nous avons analysé les produits qui nous permettent de détecter les menaces et, dans certains cas, de répondre de manière autonome à ce qu'un RBS et un NIPS peuvent faire, nous avons également constaté qu'ils peuvent générer des événements, des alertes et que ceux-ci peuvent être envoyés à notre plateforme SIEM, ainsi que les journaux S.O. et middleware, y compris le trafic réseau reçu par les ordinateurs.

Avec toutes ces informations au cas où une menace deviendrait un incident, nous avons besoin d'une plate-forme pour le gérer et cette plate-forme est principalement TheHive, avec l'aide de Cortex et de MISP.

###TheHive

Hive est un outil open source qui permet de gérer les incidents de sécurité, conçu pour être utilisé par les SOC, les CSIRT, les CERT, etc. en général, quiconque souhaite gérer les incidents de sécurité qui doivent faire l'objet d'une enquête.

Il s’agit d’une solution évolutive, car elle permet une croissance horizontale du nombre de nœuds pour améliorer les performances, nécessite Java pour son fonctionnement et utilise Elasticsearch comme référentiel de données.

L'outil permet de partager des informations, de s'enrichir de sources externes (MISP) et de répondre aux incidents à l'aide d'un autre composant dont Cortex parlera plus tard.

Au moment de cette étude, la dernière version disponible est la version 4.1.0 Vous pouvez obtenir des services professionnels pour l'installation, la configuration et le support du produit, ce qui est une valeur supplémentaire pour une utilisation dans les organisations.

Le produit peut être implanté sur une machine physique ou virtuelle (IaaS) ou contaminé par Docker (CaaS).

###Capacités de TheHive

Comme résumé sur le site du projet TheHive, voici les fonctionnalités de base du produit.

Collaboration :

•      Plusieurs analystes SOC et CERT peuvent collaborer simultanément sur les investigations

•      Grâce à la diffusion en direct intégrée, les informations en temps réel relatives aux cas, tâches, observables et CODES nouveaux ou existants sont disponibles pour tous les membres de l'équipe.

•      Les notifications spéciales leur permettent de gérer ou d'affecter de nouvelles tâches et d'afficher un aperçu des nouveaux événements MISP et des alertes provenant de sources multiples, telles que les rapports par e-mail, les fournisseurs CTI et SIEM. Ils peuvent ensuite les importer et les examiner immédiatement.

Élaboration :

•      Les cas et les tâches associées peuvent être créés à l'aide d'un moteur de modèle simple mais puissant.

•      Vous pouvez ajouter des mesures et des champs personnalisés à vos modèles pour stimuler l'activité de votre équipe, identifier les types d'investigations qui prennent beaucoup de temps et rechercher l'automatisation des tâches fastidieuses à l'aide de tableaux de bord dynamiques.

•      Les analystes peuvent enregistrer leur progression, joindre des preuves ou des fichiers notables, ajouter des balises et importer des fichiers ZIP protégés par mot de passe contenant des programmes malveillants ou des données suspectes sans les ouvrir.

Action :

•      Ajoutez un, des centaines ou des milliers d'observateurs à chaque cas que vous créez ou importez directement à partir d'un événement MISP ou de toute alerte envoyée à la plateforme.

•      Classer et filtrer rapidement.

•      Exploitez la puissance de Cortex et de ses analyseurs et répondeurs pour obtenir des informations précieuses, accélérer vos recherches et contenir les menaces.

•      Tirez parti des étiquettes, marquez les IOC, les observations et identifiez les observations précédentes pour alimenter votre intelligence des menaces.

•      Une fois les enquêtes terminées, exportez les IOC vers une ou plusieurs instances de MISP.

Le produit TheHive fournit une API REST pour l'interaction et l'automatisation des tâches en plus de l'accès Web pour les utilisateurs interactifs.

###Cortex

Cortex est un analyseur d'observables et un moteur de réponse actif qui permet, en conjonction avec TheHive, d'aider à la phase de confinement d'un incident de sécurité.

L'outil est open source et permet d'automatiser le traitement des observables en exposant une REST API.
Bien que Cortex soit free, cela ne signifie pas que les analyseurs et les répondeurs qu'il inclut lors de demandes à des tiers ne nécessitent pas de paiement ou d'abonnement à l'utilisation.

###Capacités de Cortex

Cortex peut être utilisé de manière autonome en utilisant son « interface » Web ou en association avec la plateforme de gestion des incidents TheHive, comme indiqué ci-dessus, il permet d'automatiser et d'exécuter simultanément des analyseurs et des répondeurs pour plusieurs cas.


Analyseurs/répondeurs :

•      Vous disposez d'un grand nombre d'analyseurs ou vous pouvez créer vos propres analyseurs, ainsi que des répondeurs utilisant n'importe quel langage de programmation qui peut courir sur Linux.

•      Ces analyseurs ou répondeurs peuvent être disponibles pour l'ensemble de l'équipe (cela est utile pour plusieurs raisons, chaque membre de l'équipe n'a pas à effectuer ses propres développements et peut partager des API KEYS de services externes qui peuvent être libres ou payants.

•      Permet d'interroger plusieurs instances MISP (composant que nous aborderons plus tard).

Interaction :

•      TheHive peut se connecter à une ou plusieurs instances de Cortex et peut analyser un grand nombre d'observables à la fois, ainsi que donner des réponses actives.

•      Avec le moteur de rapport TheHive, il est facile d'analyser la sortie de Cortex et de l'afficher comme vous le souhaitez.

•      Vous pouvez également utiliser Cortex comme produit autonome en utilisant votre interface utilisateur Web pour gérer plusieurs organisations, analyseurs et définir des limites de requêtes.

•      Cortex peut interagir avec d'autres produits via son API REST ou via Cortex4py.

Exécution :

•      Cortex est livré avec plus d'une centaine d'analyseurs pour les services populaires tels que VirusTotal, Joe Sandbox, DomainTools, PassiveTotal, Google Safe Browsing, Shodan et Onyphe. Identifiez les contacts abusifs, analysez les fichiers dans différents formats tels que OLE et OpenXML pour détecter les macros VBA, générez des informations utiles dans les fichiers PE, PDF et bien plus encore.

•      Les analyseurs Cortex peuvent également être consultés à partir de MISP pour enrichir les événements et étendre la couverture de vos investigations.

###MISP

Il s'agit d'une plateforme open source pour le traitement des renseignements sur les menaces et d'une norme ouverte permettant de partager ces renseignements entre les organisations et les CERT, CSIRT, etc. respectifs.

La clé de ce produit est l'automatisation, permettant de partager les indicateurs d'engagement (IoCs) avec les différents outils des plateformes de détection et de réponse aux menaces (IDS, SIEM, etc.)

La plate-forme permet d'enregistrer de manière structurée les IoCs et de les partager avec d'autres MISP et leur facilité d'utilisation a été étendue.

Au moment de cette analyse, la plus grande version de MISP est la version 2.4, pour installer le produit, vous pouvez utiliser un OVA, installer sur un S.O. existant ou avec Docker, la documentation du produit comprend également de nombreuses options pour effectuer l'installation avec puppet, ansible, etc.

###Fonctionnalités MISP

L'une des principales réalisations de cet outil a été son utilisation généralisée et le partage des informations entre les organisations.
Les attaques perpétrées par des cybercriminels, et même encouragées ou soutenues à des degrés divers par les États, augmentent en nombre et en sophistication, ce qui exige que les organisations travaillent également ensemble pour se protéger contre ces menaces et que MISP est un outil qui encourage ce type d'interaction de manière automatisée, en partageant l'intelligence que chaque organisation obtient après avoir analysé et enquêté sur une attaque.
Sur le site de MISP, nous pouvons trouver ses fonctionnalités [11] :

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

###Éléments qui composent notre plate-forme SIRP

Après avoir analysé les différents composants qui pourraient faire partie d'une solution SIRP et qui ont été traités dans les sections précédentes, il est temps de choisir ce que nous allons mettre en œuvre dans notre solution et pourquoi ces composants et pas d'autres.

Tout d'abord, nous définirons les cas d'utilisation que notre plate-forme aura, ce qui facilitera le choix des composants, cela ne signifie pas que la plate-forme ne pourra pas répondre à d'autres cas d'utilisation, mais ils ne seront pas traités dans ce TFM.

####Cas d’utilisation

Les cas d'utilisation proposés pour ce TFM consistent en plusieurs caractéristiques de gestion des incidents qui testent les différents éléments de notre plateforme.

1)    Gestion d'un incident avec des logiciels malveillants détectés par l'EDR, signalés au SIEM, en créant un cas, en enquêtant sur celui-ci et en ajoutant les informations supplémentaires nécessaires jusqu'à sa résolution

2)    Gestion d'un incident avec des menaces détectables par le réseau avec notre NIPS, signalé au SIEM, création d'un dossier, enquête, ajout d'informations le cas échéant et réponse

3)    Gestion d'un incident avec des informations sur les événements recueillis par le SIEM, création d'un dossier, enquête, ajout d'informations le cas échéant et réponse

L'idée est de pouvoir gérer les incidents à l'aide des outils indiqués, avec un enrichissement des informations provenant de sources externes et, si possible, avec des réponses automatisées.

Pour la présentation et la défense du TFM, un des trois cas d'utilisation est choisi pour montrer le travail effectué et la façon dont il se comporte dans un environnement de test généré à l'utilisation et qui est documenté à partir du point 3.

###Composants de la solution

Il s'agirait des composants de notre plateforme SIRP, qui comprend, comme déjà traité, les éléments de détection et de réponse, la gestion des événements et les journaux de sécurité, ainsi que la gestion des incidents avec intégration avec des automatismes de traitement des observateurs d'incidents, ainsi que les réponses aux incidents pour améliorer la phase d'endiguement.
Nous incluons également un élément qui nous permet de partager et de collaborer avec d'autres organisations avec nos incidents analysés, déjà étudiés et

travailler et aussi enrichir nos cas avec les informations que nous pouvons recueillir.

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

###Conception de la solution

La solution pour notre plate-forme SIRP consiste à déployer les composants mentionnés ci-dessus comme s'il s'agissait d'un service de Cloud computing (SOCaaS), il s'agirait de fournir à nos clients potentiels une plate-forme complète de gestion des incidents de sécurité sur laquelle ils pourraient travailler en tant qu'utilisateurs ou déléguer entièrement à nous (votre fournisseur de services SOC et la plate-forme associée) pour effectuer toutes les tâches.

Dans ce cas, et pour simuler un environnement réel, nous aurons besoin d'éléments supplémentaires qui n'ont pas été mentionnés précédemment parce qu'ils ne font pas partie de la solution SIRP en tant que telle, comme la nécessité de disposer d'une connexion VPN entre notre site cloud et l'entreprise ou l'organisation à laquelle nous gérerons votre sécurité avec notre plateforme.
Les éléments de détection et de réponse nécessaires, tels que les agents Wazuh et les IPS, doivent être localisés sur le réseau de l'entreprise desservie par cette plateforme SIRP, ainsi que les accès nécessaires pour gérer l'installation, la réception des journaux sur la plateforme SIEM incluse dans notre solution SIRP et les autorisations permettant d'exécuter les scripts de réponse en fonction du type d'alerte détecté, le cas échéant.
Le schéma logique de la solution et de ses composants est présenté ci-dessous.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/e1966a81-5b9e-492c-ad0c-53fa00813f0f)
Illustration 7 - Schéma logique des composants

##Mise en œuvre
###Options de déploiement

Une fois que vous avez déterminé les composants qui constituent la solution, vous devez maintenant prendre des décisions concernant la mise en œuvre. Le déploiement nécessite une haute disponibilité et une solution évolutive, ce qui nécessite un haut composant d'automatisation.

Les machines physiques ont été rejetées directement parce que je ne dispose pas d'équipements suffisants pour simuler un environnement d'exploitation. La solution sera basée sur des machines virtuelles, dans mon cas en utilisant VirtualBox, mais la solution de virtualisation a priori est indifférente au montage à effectuer.

Ainsi, les multiples options d'installation des produits ainsi que leurs versions ont été évaluées, étant donné que toutes ne sont pas disponibles pour l'installation sous quelque forme que ce soit, c'est-à-dire que les dernières versions des produits mentionnées au chapitre précédent du présent document sont conçues pour faire des installations manuelles en téléchargeant les produits des sites Web de l'organisation qui parraine ou dirige le projet open source correspondant.

Pour donner un exemple pour comprendre ce qui est dit au point précédent, la version documentée à installer avec Wazuh de ELK n'est pas celle du site Web d'Elastic mais l'Opendistro Elasticsearch, bien que sur le site Web de Wazuh on traite également des options d'installation avec l'option free d'Elastic, la dernière version de ce produit qui est la  dans le cas concret de Kibana n'a pas le plugin de Wazuh.

###Installation manuelle des produits

Les différents produits disposent d'options d'installation plus ou moins documentées pour effectuer l'installation et la configuration des différents éléments via des paquets standard de SE et/ou des sources via des référentiels principalement de type GIT.

Il existe également des OVA avec des produits préinstallés et configurés sur un système d'exploitation Linux comme c'est le cas du CSIRT-KIT.

Cela peut être très utile pour un environnement de test et pour s'entraîner relativement rapidement à la fonctionnalité des produits (en particulier le CSIRT-KIT), mais comme dans notre cas nous voulons avoir l'automatisation, la haute disponibilité et l'évolutivité de la solution car cette méthode a été abandonnée.

###Installation automatisée avec Ansible

Ansible est un logiciel open source qui est devenu très populaire ces dernières années. Il permet de gérer de façon centralisée la configuration et le déploiement des applications et des produits, ce qui simplifie considérablement, grâce à des recettes appelées playbooks (dans un langage déclaratif YAML) qui utilisent des modules spécifiques, tels que la gestion des packages de système d'exploitation, la copie de fichiers et de nombreuses autres fonctionnalités, le produit comprend de nombreux modules et la communauté continue de créer de nouveaux modules. Il existe des référentiels tels que Ansible-galaxy où vous pouvez trouver une multitude d'exemples de playbooks et de rôles (concept d'ansible) pour effectuer diverses tâches dans les applications et les systèmes gérés.

Dans le cas spécifique de la mise en œuvre de la plate-forme SIRP, les produits que nous avons envisagés sur le site Web de Wazuh facilitent déjà la procédure, bien qu'il nécessite également quelques retouches permettant de faire l'installation de Wazuh et Elastic ainsi que l'installation d'agents Wazuh et Filebeat.

Pour les autres produits choisis pour la plate-forme SIRP, il existe également des Playbooks prêts à l'installation, comme cela a déjà été dit, la communauté facilite cela dans de nombreux cas via des référentiels GIT, etc.

###Installation avec Docker / Docker Compose

Docker est l'une des plates-formes de conteneurs les plus utilisées aujourd'hui, permettant de séparer les applications et l'infrastructure nécessaires à leur exécution dans ce que l'on appelle les microservices.
Le conteneur diffère entre autres de la virtualisation, car il est beaucoup plus léger, car vous n'avez pas besoin de déployer le système d'exploitation complet, mais une couche système minimale et les librairies nécessaires pour exécuter l'application/le produit.
Il vous permet de travailler avec des images précréées et les conteneurs sont créés, détruits et recréés à partir de ces images. Si une persistance est requise, les conteneurs doivent créer des volumes ou mapper des répertoires sur l'hôte sur lequel les conteneurs sont exécutés, de sorte que les données restent persistantes même si le conteneur est détruit.
Au niveau de la sécurité, ils n'exposent que les ports nécessaires qui sont visibles de l'extérieur, les autres ports ne sont visibles en interne que par le réseau de conteneurs (qui a priori est caché à l'extérieur).
Docker Compose est un outil qui permet d'« orchestrer », plutôt d'automatiser à l'aide d'une recette descriptive YAML les conteneurs à créer et avec quelles fonctionnalités et ressources (réseaux et volumes essentiellement).

Dans notre cas particulier pour la plate-forme SIRP, ce serait une bonne solution et cela nous permet d'atteindre l'objectif d'automatisation, pour la partie haute disponibilité et évolutivité, il suffirait de disposer de plus de matériel capable d'exécuter des conteneurs et d'appliquer la recette personnalisée pour créer et gérer les conteneurs nécessaires.
Il existe une solution pour les conteneurs pour Wazuh, Elasticsearch (OpenDistro, et la plus entreprise d'Elastic), ainsi que pour TheHive, Cortex et MISP, qui ne fonctionnent pas dans certains cas avec les dernières versions du produit, mais permettent d'utiliser ces images. Vous pouvez également créer votre propre image ou une image basée sur la modification d'une image existante à l'aide des fichiers de configuration Dockerfile.

###Installation au-dessus de Kubernetes

À ce stade, nous avons déjà une solution pour pouvoir déployer automatiquement les composants d'application en adaptant les « recettes » aux différents équipements, comme cela a déjà été dit au point précédent, mais pour faire une véritable orchestration des composants, nous avons Kubernetes.
Kubernetes (communément appelé k8s), comme indiqué sur votre site Web, est une plateforme portable et extensible open source pour la gestion des charges de travail et des services, tout comme avec l'option Docker-Compose, vous avez une manière déclarative de gérer le déploiement des composants (microservices), en exécutant le deploy des PODs et services nécessaires au fonctionnement de l'application ou du produit. Et tout cela, grâce à un environnement de gestion APIfigé.
Ce projet a été lancé par Google en 2014 et a depuis été popularisé au point qu'il existe plusieurs versions qui implémentent kubernetes (kubeadm, minikube, kops, etc.), chacune a des fonctionnalités de base communes (par exemple les commandes), mais elles se différencient ensuite entre les composants de base de la solution et l'application de celle-ci (par exemple minikube est généralement utilisé dans des environnements monomodes et/ou pour tester la technologie kubernetes, bien qu'il s'agisse d'une solution 100 % opérationnelle).
Kubernetes est une solution tellement flexible qui permet la portabilité entre différents fournisseurs de Cloud public (Azure, AWS, GCP, etc.) ou Cloud privé (on-premise). Il s'agit d'une plate-forme qui tente de renforcer les méthodologies agiles et le DevOps, qui est conçu pour prendre en charge le CI/CD.
Dans notre cas, nous utiliserons Docker comme plate-forme de conteneurs, mais c'est une solution tellement flexible qu'elle est a priori indépendante de la plate-forme de conteneurs choisie (CRI, Containerd ...), de multiples outils de gestion, addons, etc.
Nous avons également discuté précédemment du concept de POD, qui est l'unité minimale de Kubernetes qui serait comme une enveloppe d'un ou plusieurs conteneurs qui remplissent certaines caractéristiques.

Une fois la technologie introduite, nous allons à notre étude de cas qui est la plate-forme SIRP avec les composants déjà mentionnés. Il existe de la documentation sur l'installation de Wazuh / Elasticsearch sur la plate-forme k8s, pour tous les autres composants, bien qu'il puisse y avoir une solution au niveau de la communauté open source, ce qui peut être basé sur les implémentations effectuées directement dans Docker ou Docker-Compose pour développer notre propre recette d'installation à partir des images déjà disponibles (TheHive, Cortex et MISP).
Ainsi, avec ce système de déploiement, nous pouvons avoir une solution avec une haute disponibilité, évolutivité et automatisation dans le déploiement et la maintenance.

###Solution d'installation choisie

Après avoir examiné les différentes options disponibles pour la mise en œuvre de notre solution SIRP, il a été considéré que la meilleure option est d'utiliser la plateforme Kubernetes car elle offre tous les avantages dont un déploiement de ce type avec plusieurs composants peut avoir besoin.

Pour l'installation des produits de base de la plate-forme SIRP, comme mentionné ci-dessus Wazuh, Elasticsearch, TheHive / Cortex et MISP, nous utiliserons Kubernetes et pour le déploiement des agents, lisez agent Wazuh et les divers Beats utiliseront l'outil également commenté dans les points précédents Ansible.

Par conséquent, nous tenterons d'utiliser la documentation et les procédures fournies sur les différents sites de ces projets open source en les adaptant à notre environnement de simulation, que nous détaillons dans les points suivants de ce document.

Pour l'environnement kubernetes, il a été choisi de déployer un cluster sur HA avec Kubeadm, dans lequel nous aurons 3 nœuds master et 4 nœuds worker (tous ont l'agent kubelet installé), en terminologie kubernetes les nœuds master ou control plane sont des nœuds contenant les composants apiserver, controller-manager, scheduler et etcd (bbdd clé-valeur qui stocke les informations de cluster, les configurations, les secrets, les services, etc.).

Les nœuds maîtres, car nous pourrions dire qu'ils sont des nœuds de gestion dans ce cas de kubernetes, ont également été réutilisés pour installer Ansible

Pour des raisons de sécurité, le déploiement d'applications ou d'autres composants sur les nœuds maîtres doit être désactivé. Nous disposerons donc de 4 nœuds efficaces pour déployer notre solution SIRP.

![image](https://github.com/Ysejal/soc-devops-pro/assets/72010054/2f305055-5c5d-4e1b-92be-1ebcafc67f85)
Illustration 7 - HA kubernetes etcd stacked (kubernetes.io)

Il existe deux modèles pour monter HA de Kubernetes avec kubeadm, le modèle topologique de l'image qui est suivi dans ce projet «etcd stacked» et le modèle «etcd external».

 ###Ressources

 Dans l'environnement de simulation utilisé dans ce projet qui a été déployé, nous pouvons diviser les ressources utilisées en deux parties, celles qui composent le réseau d'entreprise à protéger et ce que serait la plateforme SIRP (simulation d'un environnement Cloud avec Kubernetes).

Toute la solution s'articule autour de 4 ordinateurs physiques dotés de processeurs Intel i7 QuadCore et de 32 Go de RAM.

###Plate-forme SIRP (simulation Cloud On-premise)

Les ressources utilisées dans la simulation de Cloud On-premise ou Cloud Privé sont les suivantes :

•      1 machine virtuelle - Pare-feu Pf-Sense (2 vCPU, 4 Go de RAM et Go d'espace disque) Logiciel :

Appliance Pf-Sense (basée sur FreeBSD)

•      7 machines virtuelles - Cluster Kubernetes :

3 VMs nœuds Master (2 vCPU et 4 Go de RAM et 100 Go de disque) ou 4 VMs nœuds Worker (2 vCPU et 8 Go de RAM et 100 Go de disque)

S.O. CentOS 8.3 ou HAProxy 1.8.23 ou Keepalived 2.0.10 ou GlusferFS 9.1
ou Docker CE 20.10.6
ou Docker-Compose 1.29.1

• Kubernetes 1.21.0 (kubectl, kubeadm, kubelet) ou Ansible 2.9.18 (nœuds Master uniquement) :
  •   Wazuh 4.1.1
  •  Opendistro Elasticsearch 1.12.0 (Elasticsearch et Kibana 7.10) ou Filebeat 7.10
ou TheHive 4.1.4 ou Cortex 3.1.1 ou MISP 2.4.142

