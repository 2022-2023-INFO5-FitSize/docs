# FitSize: Rapport Final

**L'équipe**\
*Clément NGUYEN* (chef de projet)\
*Florian CHAPPAZ*\
*Valentin DE OLIVEIRA*\
*Sami IFAKIREN*


## Sommaire

1. [Rappel du sujet](#rappel)\
    1.1. [Besoin](#besoin)\
    1.2. [Cahier des charges](#cahier)

2. [Technologies employées](#technologies)\
    2.1. [Front-end](#tech_front)\
    2.2. [Back-end](#tech_back)

3. [Architecture technique](#architecture)

4. [Réalisations techniques](#realisations)\
    4.1. [Front-end](#real_front)\
    4.2. [Back-end](#real_back)

5. [Gestion de projet](#gestion)\
    5.1. [Méthode](#methode)\
    5.2. [Planning](#planning)\
    5.3. [Rôles des membres](#roles)

6. [Outils](#outils)\
    6.1. [Collaboration](#collaboration)\
    6.2. [CI/CD](#ci_cd)

7. [Métriques logiciels](#metriques)\
    7.1. [Lignes de code](#lignes)\
    7.2. [Langages](#langages)\
    7.3. [Performance](#performance)\
    7.4. [Temps ingénieur](#temps_ingenieur)\
    7.5. [Répartition](#repartition)

8. [Conclusion](#conclusion)



## Rappel du sujet <a id='rappel'></a>

### Besoin <a id='besoin'></a>

L'industrie de la mode compte de nombreux problèmes. L'un d'entre eux est le soucis des clients à trouver du premier coup la taille qui leur correspond. 

Malgré les normes de tailles, qui de plus peuvent différer en fonction du pays, les marques peuvent tailler leurs vêtements de manières différentes. Ainsi, il peut arriver que les clients se trompent de taille en fonction de la marque.

FitSize apporte donc une solution pour palier à ces problèmes de normes confuses. FitSize est une application mobile, conçue à la fois pour les vendeurs de vêtements et pour leurs potentiels clients.

Le principe de FitSize est de permettre aux vendeurs de scanner leurs vêtements mis en vente. L'application va alors déterminer automatiquement, à l'aide d'une intelligence artificielle, les dimensions du vêtement. 

L'acheteur pourra lui aussi scanner des vêtements : ceux qui lui vont le mieux. L'application déterminera leurs mesures de la même manière, et sera ainsi capable de proposer à l'acheteur de nouveaux vêtements de taille similaire.


### Cahier des charges <a id='cahier'></a>

L'objectif du projet est de développer la phase 1 de la solution qui se compose uniquement de la création d'un logiciel pour acheteurs à destination des smartphones. Il permet de faire un relevé des mensurations sur des vêtements à plat.

Nous avons comme première base le travail effectué par le groupe de [l'année dernière](https://github.com/pfefitsize).
Cependant, le projet de l'année dernière ne s'incrivait pas tout à fait dans les mêmes objectifs. Il s'agissait d'un premier prototype, basé sur la réalité augmentée, où l'utilisateur devait lui même choisir une référence de mesure et placer les points d'ancrages autour du vêtement.

Or, dans le projet de cette année, un des grands objectifs voulus par l'entreprise est d'automatiser un maximum de tâches, afin que les utilisateurs puissent atteindre leur objectif avec un minimum de clics.

Les objectifs suivants ont été fixés par nos tuteurs en tout début de projet : 

1) Concevoir une application mobile permettant de scanner des vêtements et en constituer un catalogue.
2) Concevoir le serveur de manière à pouvoir stocker et modifier le catalogue.
3) Avec un script et des modèles donnés permettant le calcul automatique de points d'ancrages à partir de l'image d'un vêtement, le rendre accessible sur le serveur avec la création d'un endpoint dédié.
4) Restituer les dimensions calculées sur l'application mobile pour chaque vêtement scanné.

## Technologies employées <a id='technologies'></a>

Après avoir analysé ce qu'a fait le groupe de l'année dernière, nous avons choisi de garder les technologies qui ont été utilisées.

### Front-end <a id='tech_front'></a>

- **Flutter** : c'est un framework open-source créer par Google afin de développer des applications mobiles multi-plateformes. Ce framework possède de nombreux avantages. En effet, il permet de créer des applications pour plusieurs plateformes (iOS, Android, web et bureau) à partir d'un seul code source, ce qui fait gagner considérablement de temps et de coûts. De plus, Flutter utilise le langage de programmation Dart qui est compilé en code natif pour chaque plateforme, ce qui permet d'obtenir des performances élevées et une expérience utilisateur fluide.

### Back-end <a id='tech_back'></a>

- **Django** : c'est un framework Web Python qui permet le développement rapide d'applications Web sécurisées et maintenables. Il respecte l'architecture Modèle-Vue-Template (MVT).
- **Celery** : nous en reparlerons plus loin, framework pour implémentation d'un serveur asynchrone.
- **PyTorch** : pas d'utilisation directe, mais utilisé dans le script fourni de l'intelligence artificielle.


## Architecture technique <a id='architecture'></a>

**Vue physique**
![](https://i.imgur.com/srDV0vK.png)

**Diagramme de séquence**
![](https://i.imgur.com/JR0EhWW.png)

**Diagramme de classes**
![](https://i.imgur.com/PlYe6c8.png)


## Réalisations techniques <a id='realisations'></a>

### Front-end <a id='real_front'></a>

Avec l'équipe front-end, nous avons décidé de développer l'application mobile de zéro. En effet, l'équipe de l'année passée a développer une application mobile sans interactions avec le serveur, nous avons donc préféré de repartir depuis le début.
On rappelle que l'on développe la phase 1 de la solution qui se compose uniquement de la création d'un logiciel pour acheteurs à destination des smartphones. Il permet de faire un relevé de mensuration sur des vêtements à plat.
Notre application se compose principalement de deux parties : la prise de photo et le catalogue.

**> L'accueil**

L'accueil est une interface de capture de photos qui permet de prendre un vêtement en photo en choisissant le type de vêtement : t-shirt, pantalon, robe, jube ou manteau. 
Après la capture de la photo, on fait un appel à l'API keypoints afin d'obtenir les keypoints de la photo. Une fois qu'on obtient ces derniers, on calcule les dimensions du vêtement avec les keypoints à l'aide de la fonction **calculateDimensions()**. Pour calculer les dimensions à partir des différents points, on doit calculer la distance euclidienne entre les points et diviser par la distance en pixel. Nous obtenons une distance en centimètre. 
Et enfin, nous enregistrons les données du vêtement dans la base de données. 
Cette interface est codée dans le fichier **Home.dart**.


**> Le catalogue**

Une fois qu'on a pris la photo, l'image apparaît sur la page catalogue. C'est l'interface où se trouve l'ensemble des vêtements enregistrés par l'utilisateur. L'utilisateur peut voir les caractéristiques d'un vêtement en cliquant sur le vêtement présent sur sur la page.
Pour afficher les vêtements que l'utilisateur a pris, on effectue une requête GET avec cette route : /polls/usermodel/$login, login est l'argument qui correspond au nom d'utilisateur. 
La page catalogue est codée dans le fichier **Catalogue.dart** et le détail d'un vetement est dans le fichier **detailClothing.dart**


**> La page de connexion**

Cette page permet simplement de se connecter à l'application. En interne elle permet de récupérer les données de l'utilisateur comme son login afin de faire des requêtes avec l'API. Cette page n'est pas une "vraie" page de connexion car nous ne stockons pas de token d'authentification, on l'utilise afin de récupérer l'id de l'utilisateur.


### Back-end <a id='real_back'></a>

#### Pré-étude

Avant de nous lancer véritablement dans le développement du serveur, il nous a semblé nécessaire d'étudier le projet existant de l'année dernière. Même si les objectifs étaient différents, il y avait tout de même des éléments qui nous semblaient pertinents, notamment la partie permettant de gérer le catalogue, avec des tests implémentés.

Le rapport de pré-étude complet est disponible dans notre rapport technique, ou directement sur notre [GitHub](https://github.com/2022-2023-INFO5-FitSize/docs/blob/main/Etude_backend_existant.pdf).

#### Endpoints pour le catalogue

Beaucoup d'API on été fourni par le groupe de l'année dernière. Vu que des modifications ont été effectuées sur le modèle, certaines API on du être réécrite.

Il a également fallu écrire une API complète pour gérer les représentant d'entreprise. En effet, l'entreprise nous avait proposé de créer une classe pour prendre en compte un représentant d'entreprise. Un représentant pourrait publier des images et des tailles associées pour les vêtements de son entreprise. Cela a nécessité plusieurs changements pour représenter le lien entre un représentant d'entreprise et une entreprise.

À noter que les API répondaient également aux contraintes imposées par le frontend. Cela nécessitait une communication régulière entre l'équipe du front et celle du back. 

#### Endpoints pour le script IA

**> Présentation du script**

Nous n'avons pas eu a développer l'aspect de l'intelligence artificelle et l'inférence des points d'ancrages à partir d'une image. Le [code](https://github.com/2022-2023-INFO5-FitSize/FitSize---Backend/tree/main/keypoints/code), issu d'un concours, nous a directement été fourni par l'entreprise.

Cependant, ce code était assez peu documenté, et nos tuteurs ont souhaité, dans la dernière phase du projet, que nous tentions de creuser le code afin de trouver son origine, les données utilisées, et les éventuelles pistes d'améliorations.

Le rapport sur ce code est disponible sur notre [Github](https://github.com/2022-2023-INFO5-FitSize/docs/blob/main/Etude%20IA%20FitSize.pdf) ou directement dans notre rapport technique.

**> Phase 1 : serveur synchrone**

Dans un premier temps, le travail sur cette partie consistait simplement à essayer d'intégrer le script et rendre son exécution et ses résultats accessibles sur un endpoint de l'API.

Nous avons alors créé un unique endpoint à l'aide de Django. Cet endpoint, nommé execScript, est une requête POST prenant une image en base64 et le type de cet image (parmi {'trousers', 'blouse', 'outwear', 'skirt', 'dress'}). 

Vous pouvez retrouver le code associé sur ce [commit](https://github.com/2022-2023-INFO5-FitSize/FitSize---Backend/blob/2dabfbda84598a8db1c4c10cd2d277e36a93c38f/keypoints/views.py).

Une fois la requête reçue, l'image et son type son envoyé au script par le biais de la méthode *subprocess()*. Mais la requête ayant pour rôle d'envoyer les résultats, elle est bloquante jusqu'à ce que le script aie fini son exécution.


**> Phase 2 : serveur asynchrone**

La première phase d'implémentation de l'API pour l'IA était fonctionnelle. Cependant, nous souhaitions que le client soit capable de lancer une tâche de calcul sur le serveur et d'accéder aux résultats quand il le souhaite. Nous souhaitions aussi que le serveur puisse traiter plusieurs requêtes simultanées venant de clients différents de manière concurrente, afin que l'attente pour les clients soit moins longue, même avec une charge plus haute.

Le framework Python nommé **Celery** nous a permis cette implémentation asynchrone. Nous avons alors du séparer notre API en deux endpoints :
- POST execScript, pour récupérer l'image et le type de vêtement pour lancer la tâche -> retourne instantanément un numéro de tâche pour accéder plus tard aux résultats.
- GET taskStatus, pour récupérer le statut, et si possible les résultats, d'une tâche avec un id donné.

Ce fonctionnement en deux temps permet au client de rentrer quelques métadonnées le temps que la tâche se finisse.

Celery a un autre avantage : son fonctionnement repose sur un pool de workers, répartis équitablement sur le processeur, afin d'exécuter plusieurs workers de manière concurrente.


## Gestion de projet <a id='gestion'></a>

### Méthode <a id='methode'></a>

Comme décrit précédemment nous avons décidé de séparer notre groupe en deux équipes de 2 membres chacune afin de répartir la charge de travail de façon plus homogène. Ainsi chaque personne pouvait trouver le soutient d'au moins une autre personne qui serait familiere avec le meme code que celui en cours de production.

### Planning <a id='planning'></a>

Au sein de l'équipe nous avions de tres régulieres réunions pour effectuer un état des lieux de notre production. Parfois seulement entre membres front-end/back-end et a d'autres moments avec l'ensemble du groupe notamment lorsqu'il sagissait de lier les deux parties ensembles.

Nous organisions également tous les jeudis une réunion en visio-conférence avec nos tuteurs de l'entreprise Fitsize. Durant ces appels nous avions l'occasion de présenter les avancements effectués mais également celle de demander des précisions sur certains choix critiques à faire pour s'adapter au mieux à leur vision du produit final.

Le projet se décompose en 3 sprints :
* Le premier sprint a débuté le 30 janvier, il a duré 2 semaines et contient l’étude du projet FitSize du groupe de l’année dernière, la pré-étude ainsi que la formation aux différentes technologies. 
* Le second sprint a débuté après les vacances d’hiver pour une durée d’une semaine et demie et se compose d’une première mise en place du serveur et des différents API ainsi qu’un prototype.
* Le dernier sprint consistait à avoir une application fonctionnelle avec une communication avec le serveur puis à effectuer des tests. Ce sprint comprenait aussi une optimisation du serveur, en particulier de l’intelligence artificielle ainsi que sa documentation.


### Rôles des membres <a id='roles'></a>

Les affectations se sont donc faites comme suit :

**NGUYEN Clément** : Front-End, partie catalogue et intégration back
**IFAKIREN Sami** : Front-End, partie appareil photo, images et communication IA
**DE OLIVEIRA Valentin** : Back-End, API Catalogue + tests
**CHAPPAZ Florian** : Back-End (IA), API IA + Documentation


## Outils <a id='outils'></a>

### Collaboration <a id='collaboration'></a>

**Groupe Messenger**

Nous utilisions Messenger pour nous échanger de courtes informations rapidement.

**Groupe Discord**

Nous utilisions Discord pour échanger des informations plus longues, faire des appels

**Trello**
![](https://i.imgur.com/2dQVOMG.png)

**Github**
Nous avons eu recours a github pour faciliter notre collaboration et surtout l'organisation de notre production. Notamment via la création d'une branche pour chaque nouvelle fonctionnalité majeure. 

**Réunions**

Nous organisions une réunion avec nos tuteurs toutes les semaines, le jeudi matin, 

### CI/CD <a id='ci_cd'></a>

Les différents changements apportés au modèle de l'année dernière ont cassé une partie des tests. Nous avons réécrit une très grande partie des tests. L'absence d'une véritable documentation sur les tests écrits a rendu la tâche difficile. Par manque de temps, tous les tests n'ont pas pu être réécrits et donc certains tests ne passent pas.

## Métriques logiciels <a id='metriques'></a>

### Lignes de code <a id='lignes'></a>

le nombre de lignes de codes ajoutés/modidifiés a été calculer grâce à l'outil <a href="https://github.com/arzzen/git-quick-stats">git-quick-stats</a>.

Avec cette outil, on peut savoir rapidement le nombre d'insertions et suppression de chaques utilisateur sur l'intégralité du projet. Les chiffres donnés sont à prendre avec précautions. En effet, les lignes ajoutés correspondent également aux lignes qui ont pu être généré par le langage ou par un outil tiers.

Par exemple, Florian a récupéré le modèle de l'IA fournit par l'entrpesei pour le rajouter sur le Git. Il n'a pas vraiment écrit ces lignes mais elles sont comptabilisé par l'outil. Cela représente environ 10900 lignes.

#### Résultats du Back-End

 |  | Lignes ajoutées | Lignes supprimées  
 |--- |--- |--- 
Valentin De Oliveira   | 802 | 156
Florian Chappaz        | 619 | 324 
Clément Nguyen         | 34 | 0

#### Résultats du Back-End

|  | Lignes ajoutées | Lignes supprimées  
|--- |--- |--- 
Clément Nguyen        | 1153 | 514
Sami Ifakiren         | 893  | 344 



### Langages <a id='langages'></a>

#### Back-End

![](https://i.imgur.com/y8JzbBE.png)


#### Front-End

![](https://i.imgur.com/RycLeUq.png)


### Performance <a id='performance'></a>

Nous avons tenté d'étudier les performances du serveur à l'aide de l'outil JMeter. Par manque de temps, ce rapport est incomplet mais il pose les bases. Il est disponible [ici](https://github.com/2022-2023-INFO5-FitSize/docs/blob/main/Etudes_Performances_FitSize.pdf) ainsi que dans le rapport technique.

Nous avons également analysé les performances d'une requête unique de calcul de points d'ancrage dans l'[étude de l'intelligence artificielle](https://github.com/2022-2023-INFO5-FitSize/docs/blob/main/Etude%20IA%20FitSize.pdf).

### Temps ingénieur <a id='temps_ingenieur'></a>
Calcul du temps de travail:
on considère 35h semaine pour 4 personnes sur 7 semaines pleines donc : 4x7x35 = 980h


### Répartition <a id='repartition'></a>

![](https://i.imgur.com/l6tiEU4.png)


## Conclusion <a id='conclusion'></a>

Durant ce projet, nous avons eu l'occasion de nous familiariser avec de multiples nouvelles technologies. D'une partie, nous avons appris à produire des applications pouvant être déployées sur différentes plateformes et en simultané via le framework Flutter. Dans un second temps, nous nous sommes familiarisé avec la création d'API REST performantes au travers du framework Django qui est en lien direct avec l'intelligence artificielle qui nous a été fournie.
