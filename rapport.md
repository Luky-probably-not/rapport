# Dossier de compétences — Portefeuille de preuves

_Document généré automatiquement à partir des blocs 1 à 4._

\newpage



# Bloc 1 — Concevoir et développer des applications logicielles

\newpage


## Compétence 1.1

## Activité 1.1 : Étude du cahier des charges et de l’existant. Rédaction du cahier des spécifications fonctionnelles

J'ai pu acquérir cette compétence dans le cadre de mon alternance, lors d'une évolution fonctionnelle de l'API Nomenclatures utilisée par notre BFF assuré.

Dans le cadre du développement d'une fonctionnalité permettant de lister les pays, un besoin métier a été exprimé afin d'identifier les pays considérés comme "à risque". Les valeurs de nomenclature pouvant être rattachées à un groupe, les pays à risque étaient définis comme appartenant au groupe identifié par le code 945.
Pour répondre à ce besoin, une évolution de l'API Nomenclatures était nécessaire. La demande m'a été transmise via un ticket GitLab (voir `c1.png` dans le zip de preuves).

Cette analyse m'a permis d'identifier les critères fonctionnels suivants :

- permettre de filtrer les nomenclatures par groupe
- conserver le comportement existant lorsque le filtre n'est pas renseigné
- garantir la compatibilité avec les consommateurs actuels de l'API
- permettre l'identification des pays à risque par le BFF

Cette analyse m'a permis de constater que l'endpoint existant répondait déjà à la majorité du besoin et qu'une évolution mineure suffisait. Afin de préserver la compatibilité avec les applications déjà connectées à l'API, j'ai choisi d'ajouter un nouveau paramètre optionnel nommé `codeGroupe`.  
Lorsque ce paramètre est renseigné, les résultats sont filtrés sur le groupe demandé. En son absence, le comportement historique de l'API est conservé. Cette approche a permis de répondre à la demande tout en évitant toute régression sur les usages existants.

Grâce à cette évolution, le BFF est désormais capable de récupérer la liste des pays appartenant au groupe des pays à risque. Une logique complémentaire a ensuite été mise en œuvre au niveau du BFF afin d'ajouter à chaque pays un indicateur précisant s'il est considéré comme un pays à risque ou non.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.1/`) :

- `c1.png`

- `doc.md`

- `requete.md`


---


## Compétence 1.2

## Activité 1.2 : Conception d’une application logicielle


---


## Compétence 1.3

## Bloc 1.3 — Programmation de briques et services logiciels. Conception de services métiers

J'ai acquis cette compétence lors de la conception et du développement de l'API d'ITeralis, une plateforme communautaire permettant de consulter et partager gratuitement des cours de cybersécurité en français.

Afin de faciliter la maintenance et l'évolution de l'application, j'ai conçu une architecture organisée en plusieurs projets .NET distincts :

- `ITeralis.Abstractions`
- `ITeralis.Application`
- `ITeralis.Domain`
- `ITeralis.Repository`
- `ITeralis.Utilitaires`

Chaque projet possède une responsabilité spécifique. Cette séparation permet d'isoler les contrats, la logique métier, l'accès aux données ainsi que les composants techniques transverses.

L'API a également été conçue selon une architecture modulaire. Chaque domaine fonctionnel de l'application (Cours, Matières, Catégories, Utilisateurs ou Images) est organisé suivant la même structure. Chaque fonctionnalité possède ses propres endpoints, objets de transfert de données (DTO) et composants de mapping.

Cette approche m'a permis de réutiliser un modèle de développement commun pour chaque nouveau domaine métier ajouté au projet. Par exemple, les fonctionnalités liées aux catégories ou aux images ont pu être développées en s'appuyant sur la même organisation que les fonctionnalités déjà existantes, garantissant ainsi une cohérence globale de l'application.

J'ai également veillé à séparer la logique métier des points d'entrée de l'API. Les endpoints ne contiennent pas de logique de persistance et s'appuient sur des interfaces définies dans le projet `ITeralis.Abstractions`. Ces interfaces sont ensuite implémentées dans le projet `ITeralis.Repository`.

Par exemple :

```txt
Endpoint
↓
Interface (ICoursRepository)
↓
Implémentation Repository
↓
Base de données MariaDB
```

Cette architecture présente plusieurs avantages :

- meilleure maintenabilité du projet
- séparation claire des responsabilités
- réutilisation des composants
- facilité d'ajout de nouvelles fonctionnalités
- réduction du couplage entre les différentes couches
- amélioration de la testabilité des composants

Enfin, les fonctionnalités transverses, telles que la gestion globale des exceptions ou certaines méthodes d'extension, ont été regroupées dans le projet `ITeralis.Utilitaires`. Ces briques peuvent ainsi être utilisées par l'ensemble de l'application sans duplication de code.

Cette organisation m'a permis de concevoir une architecture fiable, évolutive et réutilisable, répondant aux besoins fonctionnels du projet tout en facilitant ses futures évolutions.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.3/`) :

- `abstraction.png`

- `application.png`

- `domain.png`

- `exempleEndpoint.png`

- `repository.png`

- `schema.drawio`

- `utilitaires.png`


---


## Compétence 1.4

## Activité 1.4 : Conception de services d'accès aux données

J'ai acquis cette compétence lors du développement de l'API ITeralis, une plateforme permettant la consultation et le partage de cours de cybersécurité.

L'application nécessite la gestion de plusieurs types de données, notamment les cours, les matières, les catégories, les utilisateurs et les images. Afin de faciliter la maintenance de l'application et de limiter le couplage avec la base de données, j'ai conçu une couche d'accès aux données indépendante de la logique métier.

Pour cela, j'ai mis en œuvre le pattern Repository associé à Entity Framework Core et à une base de données MariaDB.

Chaque domaine fonctionnel possède sa propre interface définissant les opérations disponibles, comme par exemple :

- ICoursRepository
- IUtilisateurRepository
- IMatiereRepository
- ICategorieRepository
- IImageRepository

Ces interfaces sont définies dans le projet ITeralis.Abstractions tandis que leur implémentation concrète est réalisée dans le projet ITeralis.Repository.

Cette organisation permet aux couches métier et applicatives de manipuler uniquement les interfaces sans dépendre directement de la technologie de stockage utilisée.

```txt
Application
↓
ICoursRepository
↓
CoursRepository
↓
Entity Framework Core
↓
MariaDB
```

Cette architecture présente l'avantage de rendre le système indépendant du moteur de stockage. Si la base de données devait évoluer dans le futur, seule la couche Repository devrait être adaptée, sans impact sur la logique métier ou les endpoints.

J'ai également pris en compte la sécurité et l'intégrité des données à plusieurs niveaux.

Au niveau applicatif, des contrôles sont réalisés avant l'écriture des données afin de vérifier leur validité et de détecter les cas incohérents. Des exceptions métier dédiées permettent également de signaler précisément les erreurs rencontrées.

Au niveau de la base de données, plusieurs contraintes garantissent la cohérence des informations stockées :

- clés primaires
- clés étrangères
- contraintes NOT NULL
- contraintes UNIQUE sur certains champs comme l'adresse électronique ou le nom d'utilisateur

Enfin, l'ensemble des accès aux données est réalisé via Entity Framework Core et LINQ. Les requêtes SQL sont générées automatiquement par l'ORM et paramétrées par défaut, ce qui limite les risques liés aux injections SQL.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.4/`) :

- `Thumbs.db`

- `dbContext.png`

- `impl#U00e9mentation.png`

- `methodes.png`

- `script.png`


---


## Compétence 1.5

## Activité 1.5 : Préparation des jeux de tests unitaires

J'ai acquis cette compétence lors de la réalisation du CatchErrorBuilder, une bibliothèque utilisée dans les API BFF de l'entreprise afin de simplifier et centraliser la gestion des erreurs.

Ce composant ayant pour objectif de gérer automatiquement plusieurs situations d'échec, il était indispensable de vérifier son comportement dans l'ensemble des cas rencontrés en production.

Avant de concevoir les tests unitaires, j'ai analysé les différents scénarios fonctionnels pouvant se produire lors de l'exécution d'un traitement.

Dans notre système, plusieurs exceptions métier existent et sont associées à des codes de retour HTTP spécifiques :

- ValidationException → 400 (Bad Request)
- AccesNonAutoriseException → 401 (Unauthorized)
- DonneeNonTrouveException → 404 (Not Found)
- NonRecuperableException → 500 (Internal Server Error)

Une difficulté particulière concernait les ClientException, générées lorsqu'un appel vers une API externe échoue. Dans ce cas, l'exception métier d'origine est encapsulée dans une exception générique, ce qui peut conduire à la perte du code de retour initial. Par exemple, une ValidationException provenant d'une API distante pouvait être transformée en erreur 500 alors que la réponse attendue était un code 400.

Afin de valider le comportement du composant dans toutes les situations identifiées, j'ai préparé plusieurs scénarios de tests :

- exécution d'un traitement sans erreur ;
- exécution d'un traitement générant une exception métier locale ;
- exécution d'un traitement provoquant une erreur lors d'un appel à une API externe ;
- exécution d'un traitement avec une erreur explicitement prévue et gérée par le développeur ;
- vérification de la restitution correcte du code HTTP associé à chaque exception.
- exécution du builder sans définition préalable du traitement à exécuter ;

Cette démarche m'a permis de disposer d'un référentiel de validation clair et de vérifier objectivement le comportement attendu du composant après chaque modification. Cette préparation m'a permis de couvrir aussi bien les cas nominaux que les cas d'erreur et les situations particulières pouvant être rencontrées en environnement réel.

La définition préalable de ces jeux de tests a contribué à sécuriser le développement du composant et à garantir un comportement cohérent quelles que soient les exceptions rencontrées, facilitant ainsi son intégration dans les différentes API de l'entreprise tout en limitant les risques de régression lors des évolutions futures.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.5/`) :

- `resultats.png`

- `tests.md`


---


## Compétence 1.6


> ⚠️ **Contenu manquant.** Aucun texte n'a encore été rédigé pour la compétence 1.6. À compléter avant la génération du PDF final.


---


## Compétence 1.7

## Activité 1.7 : Détermination du nombre de tiers de l'application

J'ai acquis cette compétence lors de la conception de l'architecture d'ITeralis, une plateforme permettant la consultation et le partage de cours de cybersécurité.

Dès la phase de conception, j'ai défini une architecture reposant sur plusieurs tiers distincts afin de répartir les responsabilités de l'application et d'anticiper une éventuelle augmentation du nombre d'utilisateurs.

L'application est composée de :

- un tiers de présentation correspondant à l'application web ;
- un tiers applicatif correspondant à l'API .NET ;
- un tiers de données reposant sur MariaDB ;

Cette séparation permet de répartir les traitements en fonction de leur nature. Les interfaces utilisateur sont gérées par le clients web, la logique métier est centralisée dans l'API et les données sont stockées dans une base dédiée.

```txt
Application Web
      |
      v
     API
      |
      v
    MariaDB
```

Lors de cette conception, j'ai également isolé les différents composants dans des conteneurs Docker distincts. Cette approche permet de faire évoluer indépendamment chaque tiers en fonction de la charge observée.

L'API a été conçue sous la forme d'un service REST sans état (stateless). Ce choix facilite la montée en charge puisque plusieurs instances de l'API pourraient être déployées simultanément sans modification de la logique applicative. De la même manière, l'ajout d'un nouveau client consommant l'API ne nécessite pas la création d'un nouveau backend spécifique.

J'ai également mis en place une séparation réseau entre les services. La base de données n'est accessible que par l'API et n'est jamais exposée directement aux applications clientes. Cette organisation limite les accès directs aux données et réduit la charge supportée par le tiers le plus sensible du système.

Cette architecture me permet d'anticiper les évolutions futures de la plateforme en répartissant les traitements entre plusieurs tiers spécialisés et en conservant la possibilité d'augmenter indépendamment les ressources allouées à chacun d'eux.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.7/`) :

- `Thumbs.db`

- `api.png`

- `compose.png`

- `web.png`


---


## Compétence 1.8

## Activité 1.8 : Réalisation d'une interface homme/machine (IHM) adaptative aux situations de handicap

J'ai acquis cette compétence lors du développement d'Ecocoffee, un site vitrine e-commerce réalisé en Vue 3. Contrairement à mes projets principalement orientés API, ce projet m'a permis d'intervenir directement sur l'ensemble des interfaces utilisateur et d'appliquer les principes d'accessibilité numérique dès la phase de développement.

L'objectif était de concevoir une interface utilisable par le plus grand nombre, y compris par les personnes utilisant des technologies d'assistance telles que les lecteurs d'écran ou la navigation exclusivement au clavier.

Pour cela, j'ai conçu les différentes pages en utilisant un balisage HTML sémantique adapté. Les éléments de structure tels que header, nav, main, section, article ou footer ont été utilisés selon leur rôle réel afin de permettre aux technologies d'assistance de restituer correctement l'organisation de la page.

Afin d'améliorer la navigation au clavier, j'ai mis en place des liens d'évitement ("skip links") placés en début de page. Ces liens permettent d'accéder directement au contenu principal ou au formulaire de contact sans parcourir l'ensemble des éléments de navigation.

J'ai également structuré les différentes régions de la page à l'aide de l'attribut aria-labelledby. Chaque section est ainsi associée à un titre explicite permettant aux utilisateurs de lecteurs d'écran d'identifier rapidement le contenu proposé.

Une attention particulière a été portée aux formulaires. Pour chaque champ, j'ai associé un label explicite et utilisé des groupes de champs (fieldset et legend) lorsque cela était pertinent. Les erreurs de saisie sont annoncées de manière accessible grâce aux attributs aria-invalid, aria-describedby et au rôle alert, permettant à l'utilisateur d'être informé immédiatement d'une anomalie.

J'ai également différencié les images décoratives des images porteuses d'information. Les éléments purement visuels sont ignorés par les technologies d'assistance grâce à l'utilisation de alt="" et aria-hidden="true". À l'inverse, les informations utiles sont systématiquement restituées sous forme de contenu textuel structuré afin d'assurer leur accessibilité aux personnes malvoyantes ou non-voyantes.

Afin de prendre en compte les besoins des personnes sensibles aux animations, j'ai ajouté une gestion de la préférence système prefers-reduced-motion. Lorsque cette option est activée, les animations du site sont fortement réduites afin de limiter les risques d'inconfort liés aux mouvements.

Enfin, j'ai respecté plusieurs bonnes pratiques fondamentales d'accessibilité comme :

- la déclaration de la langue principale du document
- une hiérarchie cohérente des titres
- la conservation d'indicateurs de focus visibles
- une navigation complète au clavier

L'accessibilité a ainsi été prise en compte dès la conception de l'interface et non comme une correction ajoutée après développement. Cette démarche a permis de produire une interface plus inclusive, conforme aux recommandations du RGAA et des WCAG.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.8/`) :

- `app.md`

- `index_langFr.png`

- `labels.md`


---


## Compétence 1.9

## Activité 1.9 : Estimation, qualification des risques de sécurité

J'ai acquis cette compétence dans le cadre de l'évolution du système RBAC (Role Based Access Control) de l'entreprise.

Lors de cette évolution, les permissions précédemment définies dans le code source ont été externalisées dans des fichiers JSON afin de permettre une configuration plus flexible et de faciliter leur maintenance.

Cette nouvelle architecture apporte de nombreux avantages, mais elle introduit également de nouveaux risques de sécurité et de disponibilité. J'ai donc procédé à une analyse des risques associés à l'utilisation de ces fichiers de configuration.

J'ai notamment identifié le risque lié à la présence d'un fichier JSON invalide ou corrompu. En effet, si un fichier de permissions devient illisible à la suite d'une erreur de syntaxe ou d'une modification incorrecte, l'API n'est plus en mesure de déterminer correctement les permissions associées à un utilisateur. Cette situation peut entraîner des erreurs d'exécution et empêcher l'accès à certaines fonctionnalités.

J'ai évalué ce risque comme étant de criticité élevée en raison de son impact potentiel sur le fonctionnement du système d'autorisation de l'application.

Afin de réduire ce risque, j'ai proposé la mise en place d'un mécanisme de validation automatique des fichiers JSON lors des pipelines CI/CD. Cette validation pourrait s'appuyer sur un schéma JSON permettant de vérifier la structure, les types et les champs obligatoires avant tout déploiement d'une modification.  
Actuellement, lorsqu'une anomalie est détectée lors du chargement ou de l'interprétation des fichiers JSON, l'API renvoie une erreur empêchant l'utilisation du système d'autorisation jusqu'à la correction du fichier concerné.  
Afin de faciliter les contrôles lors des évolutions du RBAC, j'ai également développé un endpoint permettant de visualiser l'ensemble des permissions associées à chaque profil utilisateur. Cette fonctionnalité permet de comparer rapidement les droits attendus avec les droits effectivement générés et constitue un moyen supplémentaire de détecter d'éventuelles anomalies après une modification de configuration.

Cette analyse a permis d'identifier plusieurs mesures préventives destinées à améliorer la sécurité, la fiabilité et la maintenabilité du système d'autorisation tout en limitant les risques d'erreur humaine lors des évolutions futures.

| Risque                           | Impact                                 | Criticité | Mesure préventive            |
| -------------------------------- | -------------------------------------- | --------- | ---------------------------- |
| JSON invalide                    | RBAC indisponible                      | Élevée    | Validation JSON Schema CI/CD |
| Permission manquante             | Refus d'accès inattendu                | Moyenne   | Contrôle automatique         |
| Cascade des permissions invalide | Erreur de construction des permissions | Élevée    | Validation structurelle      |
| Permissions dupliquées           | Attribution incorrecte des permissions | Moyenne   | Contrôle des doublons        |

[TODO] schéma fonctionnement RBAC
Fichier JSON
        |
        v
Chargement RBAC
        |
        +--> Contrôle structure
        |
        +--> Construction permissions
        |
        +--> Endpoint de vérification
        |
        v
Contrôle accès utilisateur


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.9/`) :

- `permissions_utilisateur.json`

- `struct_permissions.json`


---


## Compétence 1.10

## Activité 1.10 : Amélioration de la qualité du logiciel et du code produit

J'ai acquis cette compétence lors du développement du CatchErrorBuilder, une bibliothèque mise à disposition des différentes API BFF de l'entreprise afin de centraliser et standardiser la gestion des erreurs.

Avant la mise en place de cette solution, chaque endpoint implémentait sa propre logique de gestion des exceptions. Cette approche entraînait une duplication de code importante ainsi qu'un risque d'incohérence dans les réponses retournées aux consommateurs des endpoints.

Afin d'améliorer la qualité du code produit, j'ai conçu le CatchErrorBuilder en appliquant les principes de la programmation orientée objet. Cette centralisation permet également de garantir un comportement homogène face aux erreurs et de limiter les risques d'oublis ou d'implémentations incohérentes entre les différents endpoints.

Cette approche présente plusieurs avantages :

- réduction de la duplication de code ;
- amélioration de la lisibilité des traitements ;
- centralisation de la logique de gestion des exceptions ;
- simplification de la maintenance ;
- uniformisation des réponses d'erreur produites par les endpoints.

Le composant a été conçu afin d'être réutilisable dans plusieurs applications. Ainsi, lorsqu'une évolution ou une correction doit être effectuée sur la gestion des erreurs, celle-ci est réalisée à un seul endroit puis bénéficie automatiquement à l'ensemble des applications utilisant cette bibliothèque.

Cette mutualisation a permis d'améliorer la robustesse globale des API tout en facilitant leur évolution future. Elle contribue également à maintenir une architecture cohérente, un niveau de qualité homogène entre les différents projets de l'entreprise et à réduire significativement la duplication de code.

Le développement de cette bibliothèque a également respecté les conventions de nommage et les standards de développement utilisés au sein de l'entreprise afin de faciliter sa compréhension et sa maintenance par les autres développeurs.

Aujourd'hui, le composant est utilisé dans l'API BFF Assuré. Son architecture a été pensée pour être réutilisable dans les autres API BFF de l'entreprise, permettant ainsi de mutualiser la gestion des erreurs et de conserver un comportement homogène entre les différents services.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.10/`) :

- `utilisation.md`


---


## Compétence 1.11

## Activité 1.11 : Programmation de l'accès aux données de l'entreprise

J'ai acquis cette compétence lors de la conception et du développement de la base de données d'ITeralis, une plateforme communautaire permettant la consultation et le partage de cours de cybersécurité.

L'application manipule plusieurs types de données métier tels que les utilisateurs, les cours, les matières, les catégories, les images ainsi que les droits associés aux comptes utilisateurs. Afin de garantir la fiabilité des informations stockées et d'éviter toute corruption de la base de données, j'ai mis en place plusieurs mécanismes de sécurité et d'intégrité.

J'ai tout d'abord conçu le schéma relationnel de la base MariaDB en définissant des contraintes d'intégrité directement au niveau des tables. Chaque entité possède une clé primaire permettant d'assurer l'unicité des enregistrements.

J'ai également défini plusieurs clés étrangères afin de garantir la cohérence des relations entre les données. Par exemple, un cours doit obligatoirement être associé à une matière et un utilisateur existants. Il est donc impossible de créer un enregistrement faisant référence à une donnée inexistante.

Par ailleurs, certaines contraintes d'unicité ont été ajoutées afin d'empêcher les doublons sur les données sensibles de l'application. C'est notamment le cas des champs `UserName` et `Email`, qui doivent être uniques pour chaque utilisateur.

Afin d'améliorer la traçabilité des opérations réalisées sur la base, j'ai également mis en place des mécanismes automatiques de suivi des modifications. Les colonnes `created_at` et `updated_at` sont alimentées automatiquement par MariaDB grâce aux clauses `DEFAULT CURRENT_TIMESTAMP` et `ON UPDATE CURRENT_TIMESTAMP`, permettant de connaître la date de création et de dernière modification de chaque enregistrement sans intervention du code applicatif.

La sécurisation des accès a également été prise en compte lors de la conception de la solution. Plutôt que d'utiliser le compte administrateur de la base de données, j'ai créé un utilisateur applicatif dédié disposant uniquement des droits nécessaires au fonctionnement de l'application (`SELECT`, `INSERT`, `UPDATE` et `DELETE`). Cette approche applique le principe du moindre privilège et limite les conséquences potentielles d'un accès non autorisé à l'API.

Enfin, au niveau applicatif, les erreurs liées aux accès aux données sont contrôlées et converties en exceptions métier dédiées. Par exemple :

- `UtilisateurUniqueClauseException`
- `AucunCoursTrouveException`
- `MisAJourCoursImpossibleException`

Cette approche permet d'éviter l'exposition directe des erreurs SQL aux utilisateurs tout en fournissant des messages cohérents et exploitables par l'application.

L'ensemble de ces mécanismes contribue à garantir l'intégrité des données, à limiter les risques de corruption de la base et à sécuriser les accès aux informations manipulées par la plateforme ITeralis.


**Preuves associées** (voir `preuves_completes.zip/bloc1/1.11/`) :

- `Thumbs.db`

- `diagramme.png`

- `exceptionUnique.png`

- `exceptionmiddleware.png`

- `init.sh`


---



# Bloc 2 — Assurer la maintenance corrective et évolutive d'une application logicielle

\newpage


## Compétences 2.1 & 2.2

## Activités 2.1 et 2.2 : Étude des procédures existantes, identification des procédures en place et contrôle de leur conformité avec la gouvernance de l'entreprise

J'ai acquis ces compétences dans le cadre de mon alternance lors du développement de plusieurs évolutions sur le **BFF Assuré**.

Avant toute intervention sur une application, l'entreprise applique un processus de développement permettant d'assurer la traçabilité des demandes, la qualité des développements réalisés ainsi que la sécurisation des mises en production.

Au cours de mes missions, j'ai d'abord étudié les procédures utilisées par l'équipe afin d'en comprendre les différentes étapes, les acteurs impliqués ainsi que les points de contrôle mis en place.

Chaque évolution suit un cycle de développement défini :

```text
Ticket GitLab
        ↓
Analyse du besoin
        ↓
Développement sur une branche dédiée
        ↓
Merge Request
        ↓
Code Review
        ↓
Validation
        ↓
Déploiement sur environnement de recette
        ↓
Recette fonctionnelle
        ↓
Mise en production
```

L'étude de ce processus m'a permis d'identifier les rôles des différents intervenants :

- développeurs
- Team Leaders
- Business Analysts

J'ai également pu comprendre les objectifs de chaque étape de validation et les mécanismes permettant de garantir la qualité des livrables.

Une fois ces procédures identifiées, j'ai veillé à les appliquer systématiquement lors de chacune de mes réalisations afin de garantir leur conformité avec les règles de gouvernance de l'entreprise.

Chaque évolution m'était attribuée au travers d'un ticket GitLab contenant :

- la description du besoin
- les critères d'acceptation
- les règles fonctionnelles à respecter

Les développements étaient ensuite réalisés dans une branche dédiée afin d'assurer la traçabilité des modifications. Une fois le travail terminé, une Merge Request était créée puis soumise à une revue de code.

Cette revue permettait notamment de vérifier :

- le respect des conventions de développement
- le respect de l'architecture applicative
- la conformité du code avec le besoin exprimé
- l'absence d'anomalies identifiables avant intégration

Les remarques formulées lors de cette étape devaient être prises en compte avant la validation définitive de la Merge Request.

Une fois validées, les évolutions étaient déployées sur un environnement dédié afin d'être testées par les Business Analysts lors de la phase de recette. Cette étape permettait de confirmer la conformité fonctionnelle de la solution développée avant son intégration dans le système d'information.

En appliquant systématiquement ce processus, j'ai participé au respect des procédures de gouvernance définies par l'entreprise et contribué à garantir la qualité, la traçabilité et la conformité des évolutions réalisées.

Cette expérience m'a permis de comprendre l'importance des procédures dans le pilotage des développements et de mesurer leur rôle dans la maîtrise des risques liés aux projets informatiques.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.1_2.2/`) :

- `codeReview.png`

- `commits.png`

- `deploiementDev.png`

- `mergeRequest1.png`

- `mergeRequest2.png`

- `ticketGit.png`


---


## Compétence 2.3

## Activité 2.3 : Reconfiguration de processus

J'ai acquis cette compétence dans le cadre d'une évolution de l'api RBAC utilisé au sein de l'entreprise.

Avant cette évolution, l'ensemble des permissions étaient définies directement dans le code source de l'application à l'aide de constantes. Chaque ajout, suppression ou modification d'une permission nécessitait une intervention d'un développeur ainsi qu'un redéploiement de l'api.

Cette approche présentait plusieurs inconvénients, tel que

- maintenance complexe
- forte dépendance aux développeurs
- risque d'erreur lors des modifications
- faible flexibilité pour faire évoluer le système d'autorisations

J'ai donc été taché de modifier ce processus en externalisant les permississions dans des fichiers de configurations JSON.

Cette évolution a permis de séparer les données de configuration de la logique applicative. Les permissions ne sont désormais plus codées en dur dans l'application mais chargées dynamiquement au démarrage du RBAC.

Afin d'illustrer cette transformation, j'ai réalisé une comparaison entre l'ancien et le nouveau fonctionnement.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.3/`) :

- `apres.md`

- `avant.md`


---


## Compétence 2.4

## Activite 2.4 : Recensement des documents utilisés, cartographie de leur circulation

J'ai acquis cette compétence dans le cadre de mon alternance sur le projet BFF Assuré.

Lors du développement d'une évolution fonctionnelle, plusieurs documents et outils interviennent tout au long du cycle de vie de la demande. Afin de comprendre le fonctionnement du projet et les interactions entre les différents intervenants, j'ai étudié la circulation de ces informations au sein de l'équipe.

Le processus débute par la rédaction des Spécifications Fonctionnelles Générales (SFG) puis des Spécifications Fonctionnelles Détaillées (SFD) par les Business Analysts. Ces documents décrivent le besoin métier, les règles de gestion, les critères d'acceptation ainsi que les impacts attendus sur les applications concernées.

Une fois ces spécifications validées lors d'un échange réunissant les Business Analysts, les Team Leaders techniques et développement ainsi que les développeurs concernés, une ou plusieurs User Stories sont créées dans GitLab.

Chaque User Story est ensuite découpée en plusieurs tickets techniques afin de répartir les travaux entre les différentes couches du système d'information. Par exemple, une fonctionnalité peut nécessiter :

- La création ou la modification des permissions du RBAC ;
- Des évolutions au niveau du proxy ;
- Des modifications sur les différentes couches applicatives (NR2, BPM ou BFF).

Une fois le ticket attribué, le développeur réalise les modifications dans une branche dédiée. À la fin du développement, une Merge Request est créée afin de soumettre les changements à une revue de code.

Les membres de l'équipe technique analysent alors les modifications proposées et formulent, si nécessaire, des remarques ou demandes de correction. La Merge Request constitue ainsi un point de validation permettant de garantir la conformité du développement aux standards de l'entreprise.

Après validation, le développement est déployé dans un environnement dédié à la fonctionnalité développée. Les Business Analysts réalisent ensuite les opérations de recette fonctionnelle afin de vérifier la conformité du livrable avec les spécifications initiales.

Une fois la recette validée, la fonctionnalité est intégrée dans la prochaine mise en production conformément aux procédures de déploiement de l'entreprise.

Cette analyse m'a permis d'identifier les différents documents intervenant dans le cycle de développement (SFG, SFD, User Stories, tickets GitLab, Merge Requests et comptes-rendus de recette), leurs auteurs, leurs destinataires ainsi que leurs points de validation respectifs.

Note Annexe
Les étapes de revue de code, recette et mise en production font partie du workflow standard de l'équipe. Bien que je ne dispose plus de captures de ces étapes pour cette évolution, elles ont été intégrées au diagramme de circulation documentaire afin de représenter le processus effectivement suivi au sein du projet.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.4/`) :

- `SFD.docx`

- `SFG.docx`

- `UserStory.png`

- `schema.drawio`

- `ticket1.png`

- `ticket2.png`


---


## Compétence 2.5

## Activité 2.5 : Conception d'une base de données

J'ai acquis cette compétence lors de la conception de la base de données du projet ITeralis.

Afin de répondre aux besoins fonctionnels de l'application, j'ai conçu un modèle de données permettant de gérer les utilisateurs, les cours, les matières, les catégories, les images ainsi que les droits associés à chaque utilisateur.

La première étape a consisté à identifier les principales règles de gestion de l'application. Un cours doit être rattaché à une matière, une matière appartient à une catégorie, un utilisateur peut être l'auteur de plusieurs contenus et chaque utilisateur possède un ensemble de droits permettant de déterminer les actions qu'il est autorisé à réaliser.

À partir de ces règles métiers, j'ai construit un modèle relationnel composé des entités suivantes :

- Utilisateurs
- Droits
- Images
- Catégories
- Matières
- Cours

J'ai volontairement séparé la gestion de l'identité des utilisateurs de la gestion de leurs autorisations. Les informations d'authentification sont stockées dans la table Utilisateurs tandis que les permissions sont regroupées dans une table Droits dédiée. Cette séparation permet de distinguer clairement les informations relatives à l'utilisateur des droits qui lui sont attribués.

J'ai également intégré plusieurs besoins fonctionnels propres à la plateforme :

- un compteur de vues sur les cours et les matières afin de mesurer leur popularité
- l'association d'une image à une matière afin d'améliorer sa présentation visuelle

Lors de la conception du modèle, j'ai également pris en compte les évolutions futures du projet. L'ensemble des entités utilise des identifiants de type UUID plutôt que des clés auto-incrémentées. Cette approche facilite notamment les échanges de données entre environnements et limite les risques de collision lors d'une éventuelle réplication des données.

Enfin, les relations entre les différentes entités ont été formalisées au travers de clés primaires et de clés étrangères garantissant la cohérence des informations stockées dans la base.

Cette modélisation m'a permis de concevoir une base de données adaptée aux besoins fonctionnels de l'application tout en conservant une structure cohérente, évolutive et maintenable.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.5/`) :

- `diagramme.png`

- `init.sh`


---


## Compétence 2.6

## Activité 2.6 : Conception de l'architecture applicative

J'ai acquis cette compétence lors de la conception de l'architecture de l'application ITeralis.

Dès le début du projet, j'ai souhaité mettre en place une architecture structurée en couches afin de séparer clairement les responsabilités de chaque composant et de faciliter les évolutions futures de l'application.

Pour cela, j'ai organisé la solution en plusieurs projets .NET distincts :

- ITeralis.Domain
- ITeralis.Abstractions
- ITeralis.Repository
- ITeralis.Application
- ITeralis.Utilitaires

Chaque couche possède un rôle bien défini.

La couche Domain contient les entités métier de l'application. Elle constitue le cœur de l'architecture et ne dépend d'aucune technologie particulière.

La couche Abstractions regroupe les contrats utilisés par les autres composants de l'application, notamment les interfaces permettant l'accès aux données.

La couche Repository assure la communication avec la base de données MariaDB et implémente les contrats définis dans Abstractions.

La couche Application contient les endpoints exposés par l'API ainsi que les objets d'échange utilisés par les consommateurs de l'application.

Enfin, la couche Utilitaires centralise les composants techniques réutilisables, tels que la gestion des exceptions ou certaines méthodes d'extension.

L'ensemble de ces couches suit une direction de dépendance maîtrisée, empêchant une couche basse de dépendre d'une couche supérieure. Cette organisation limite le couplage entre les composants et améliore la maintenabilité globale de l'application.

J'ai également adopté une organisation modulaire basée sur le concept de Feature. Chaque domaine métier (Cours, Matières, Catégories, Utilisateurs ou Images) est structuré selon le même modèle et possède ses propres endpoints, DTOs et composants associés.

Cette approche facilite l'ajout de nouvelles fonctionnalités sans remettre en cause l'architecture existante. Par exemple, l'ajout du module de gestion des images a pu être réalisé en suivant la même structure que les autres domaines déjà présents dans l'application.

L'API constitue le point central du système d'information ITeralis. Elle expose un contrat unique documenté via Swagger et est utilisée aussi bien par l'application web que par l'application mobile Flutter. Cette architecture permet de mutualiser la logique métier et d'éviter le développement de backends spécifiques pour chaque client.

Cette organisation en couches et en composants réutilisables m'a permis de concevoir une architecture applicative cohérente, évolutive et adaptée aux besoins actuels et futurs du projet.

### Preuves

#### Annexe 1 : Architecture globale d'ITeralis
- Diagramme d'architecture applicative ;
- Relations entre les différents projets.

#### Annexe 2 : Structure de la solution
- ITeralis.sln ;
- fichiers `.csproj` ;
- références entre projets.

#### Annexe 3 : Organisation par Features
Exemples :
- Features/Cours ;
- Features/Matieres ;
- Features/Categories ;
- Features/Images.

#### Annexe 4 : Contrat commun des endpoints
- `IEndpointBuilder`
- implémentations associées.

#### Annexe 5 : Composition de l'application
- `Program.cs`
- injection de dépendances ;
- chargement des modules.

#### Annexe 6 : Contrat exposé aux clients
- DTOs ;
- Swagger/OpenAPI ;
- consommation par le front web et l'application mobile.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.6/`) :

- `chargement.md`

- `organisation.png`

- `schemaSolution.drawio`

- `swagger.json`


---


## Compétence 2.7

## Activité 2.7 : Conception de la solution logicielle

J'ai acquis cette compétence lors du développement du CatchErrorBuilder, un composant destiné à améliorer et standardiser la gestion des erreurs au sein du BFF Assuré.

Le BFF communique avec plusieurs APIs du système d'information, notamment le BPM et NR2. Dans notre architecture, les erreurs métier sont représentées par différents types d'exceptions auxquelles sont associés des codes HTTP spécifiques, les plus importantes étant :

- ValidationException → 400
- DonneeNonTrouveeException → 404
- NonRecuperableException → 500

Lorsqu'un appel vers une API distante échoue, l'erreur retournée est encapsulée dans une ClientException. Cette exception ne possède aucun statut HTTP propre et n'hérite pas des exceptions métier utilisées dans l'application. Elle est donc interprétée comme une erreur générique et renvoie par défaut un code HTTP 500.

Cette situation entraînait une perte d'information fonctionnelle. Par exemple, lorsqu'une API distante retournait une DonneeNonTrouveeException, celle-ci était encapsulée dans une ClientException puis renvoyée sous forme d'une erreur 500 par le BFF, alors que le statut attendu par le consommateur était un 404.

Avant la mise en place du CatchErrorBuilder, chaque endpoint devait gérer ses exceptions au moyen de blocs try/catch dédiés. Cette approche fonctionnait correctement pour les exceptions directement levées par le BFF. En revanche, elle présentait une limite importante pour les erreurs provenant des APIs distantes.

Par exemple, le code suivant permettait de traiter correctement une DonneeNonTrouveeException locale :

```csharp

try
{
    feature();
}
catch (DonneeNonTrouveeException)
{
    return Results.NoContent();
}

```

Cependant, lorsqu'une API distante retournait une DonneeNonTrouveeException, celle-ci était transformée en ClientException avant d'arriver dans le BFF. Le bloc catch (DonneeNonTrouveeException) n'était donc jamais exécuté puisque l'exception réellement reçue était une ClientException. Cette dernière était alors traitée comme une erreur générique et renvoyait systématiquement un code HTTP 500.

Cette approche présentait donc plusieurs inconvénients :

- duplication de code dans les endpoints ;
- difficulté à maintenir un comportement homogène ;
- risque d'oublier certains cas d'erreurs ;
- impossibilité de traiter correctement les erreurs encapsulées dans les ClientException ;
- mauvaise restitution des statuts HTTP renvoyés par les APIs distantes.

Après analyse du fonctionnement existant, plusieurs solutions étaient envisageables :

- conserver le traitement manuel des exceptions dans chaque endpoint ;
- ajouter des blocs try/catch spécifiques pour chaque cas particulier ;
- modifier le middleware chargé de convertir les exceptions en ProblemDetails ;
- développer un composant dédié capable d'interpréter et de traiter l'ensemble des exceptions rencontrées.

La modification du middleware a finalement été écartée, car celui-ci intervient uniquement lors de la conversion finale des exceptions en réponse HTTP. Cette approche ne permettait pas de configurer simplement des comportements spécifiques selon les besoins de chaque endpoint.

J'ai donc retenu la dernière solution en développant le CatchErrorBuilder.

Chaque endpoint conserve sa logique métier mais délègue au builder la gestion des exceptions. Pour cela, il définit le traitement à exécuter puis les comportements spécifiques attendus pour certaines erreurs :

```csharp
return await builder
    .DefinirTraitement(() => feature())
    .OnDonneeNonTrouve(_ => Results.NoContent())
    .Executer();
```

Le rôle principal du CatchErrorBuilder est d'exécuter le traitement métier, d'intercepter les exceptions éventuelles puis de construire la réponse HTTP appropriée. Lorsqu'une ClientException est rencontrée, le composant analyse l'erreur métier qu'elle encapsule afin d'identifier le statut HTTP qui aurait dû être retourné si l'erreur n'avait pas été encapsulée.

Ainsi :

- une ClientException contenant une erreur de validation retourne un code 400 ;
- une ClientException contenant une erreur de donnée non trouvée retourne un code 404 ;
- une ClientException contenant une erreur non récupérable retourne un code 500.

Cette solution a permis de conserver le sens métier des erreurs lors des échanges entre APIs, d'éviter la perte d'information liée à l'encapsulation des exceptions et de simplifier fortement la lecture des endpoints. Le choix de développer ce composant plutôt que de gérer les exceptions individuellement dans chaque endpoint a permis de standardiser le traitement des erreurs, d'améliorer la maintenabilité du code et de garantir un comportement homogène au sein du BFF Assuré.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.7/`) :

- `AvantApres.md`

- `apres.drawio`

- `avant.drawio`

- `methodes.md`


---


## Compétence 2.8

## Activité 2.8 : Planification des tâches du projet

J'ai acquis cette compétence dans le cadre de mon alternance au sein de l'entreprise.

Afin d'assurer le suivi des différentes missions qui me sont confiées, notre équipe de développement utilise un canal Teams permettant de recenser l'ensemble des tâches réalisées, quel que soit le projet concerné. Ces informations sont ensuite utilisées pour construire un compte-rendu hebdomadaire transmis à la hiérarchie.

Chaque semaine, je réalise un tableau récapitulatif indiquant les sujets sur lesquels j'ai travaillé, leur pourcentage d'avancement ainsi que l'estimation du reste à faire exprimée en jours.

Cette activité hebdomadaire permet à l'équipe, aux Team Leaders, aux chefs de projet ainsi qu'aux responsables de disposer d'une vision globale de l'avancement des travaux. Elle permet également d'identifier d'éventuels écarts entre les estimations initiales et la charge réellement consommée, ainsi que d'anticiper les priorités des semaines suivantes.

Au fil de mon alternance, j'ai ainsi appris à :

- estimer le temps nécessaire à la réalisation d'une tâche
- mesurer l'avancement réel d'un développement
- réévaluer le reste à faire lorsqu'une difficulté est rencontrée
- communiquer régulièrement sur l'état d'avancement de mes missions
- adapter mes priorités en fonction des projets en cours

Cette démarche permet d'améliorer la visibilité sur l'ensemble des activités réalisées et contribue à une meilleure organisation du travail au sein de l'équipe.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.8/`) :

- `NR2 Suivi avancement Semaine du 30042026.msg`

- `avancement13.04.png`

- `avancement27.04.png`


---


## Compétence 2.9

## Activité 2.9 : Coordination Agile de la programmation en équipe

J'ai acquis cette compétence dans le cadre de mon alternance au sein de l'équipe de développement.

L'équipe travaille selon une organisation inspirée des méthodes Agiles. Afin d'assurer la coordination des développements en cours, des réunions régulières étaient organisées entre les différents membres de l'équipe.

J'ai participé aux réunions quotidiennes ("daily meetings") durant lesquelles chaque développeur présentait :

- les travaux réalisés depuis le précédent point
- les tâches prévues pour la journée
- les éventuels blocages rencontrés

Ces échanges permettaient de coordonner les développements, d'identifier rapidement les dépendances entre les différents sujets et de solliciter l'aide des autres membres de l'équipe lorsqu'un obstacle technique était rencontré.

Au cours de ces réunions, j'ai régulièrement présenté l'avancement de mes développements, notamment sur les projets BFF Assuré, RBAC, CatchErrorBuilder et Nomenclatures. J'ai également pu échanger avec les autres développeurs, les Team Leaders et les Business Analysts afin de clarifier certains besoins ou de confirmer des choix techniques.

Cette organisation favorisait une communication continue entre les différents acteurs du projet et permettait d'adapter rapidement les priorités en fonction de l'avancement réel des travaux.

Ces pratiques m'ont permis d'apprendre à travailler dans un environnement Agile, à communiquer efficacement sur l'état d'avancement de mes tâches et à participer à la coordination des développements réalisés par l'équipe.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.9/`) :

- `blocage.png`

- `pointDev.png`

- `pointDevBa.png`


---


## Compétence 2.10

## Activité 2.10 : Recettage du logiciel

J'ai acquis cette compétence dans le cadre d'une évolution de l'API Nomenclatures réalisée pour le projet CARENET.

Cette évolution avait pour objectif de permettre la consultation des pays appartenant à un groupe spécifique afin de répondre à un besoin de catégorisation des pays par zone géographique ou fonctionnelle.

À l'issue du développement, une phase de recette fonctionnelle a été réalisée afin de vérifier la conformité du comportement implémenté avec les spécifications exprimées par les Business Analysts.

Cette recette avait notamment pour objectif de valider :

- la récupération des données attendues ;
- le filtrage des pays selon le groupe demandé ;
- la compatibilité avec les consommateurs existants de l'API ;
- le respect du comportement attendu décrit dans les spécifications.

Une fois les vérifications effectuées, la recette a été prononcée et formalisée au travers d'un procès-verbal de vérification d'aptitude au bon fonctionnement. Ce document atteste de la conformité du développement réalisé et autorise sa poursuite dans le processus de mise en production.

Cette étape m'a permis de participer au processus de validation fonctionnelle d'une évolution logicielle et de comprendre l'importance de la formalisation de l'acceptation d'un livrable avant sa mise en exploitation.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.10/`) :

- `PV de recette_CHG0060931.docx`

- `swagger.md`


---


## Compétence 2.11

## Activité 2.11 : Démonstrations et recettage des livrables aux clients

J'ai acquis cette compétence lors de la présentation du composant CatchErrorBuilder aux responsables techniques de mon équipe.

Dans le cadre du développement de cette bibliothèque, il était nécessaire de présenter la solution afin d'expliquer son fonctionnement, les problèmes qu'elle résout et les choix de conception ayant conduit à sa mise en œuvre.

J'ai ainsi réalisé une présentation à destination de mon tuteur, Jordan, ainsi que de Florent, Team Leader Technique. À la demande de ce dernier, cette présentation a également été réalisée pour Aimée, Team Leader Développement, afin de lui présenter le fonctionnement du composant ainsi que les principes de conception utilisés.

L'un des objectifs de cette présentation était de réaliser un rappel sur le pattern Builder, utilisé comme fondement de la solution. J'ai donc présenté les principes de ce design pattern avant d'expliquer leur application concrète au sein du CatchErrorBuilder.

Afin de faciliter la compréhension du sujet, j'ai présenté le fonctionnement de la gestion des erreurs avant et après l'introduction du composant. Cette comparaison a permis de mettre en évidence plusieurs bénéfices :

- conservation du statut HTTP des erreurs provenant des APIs distantes
- réduction de la duplication du code
- amélioration de la lisibilité des endpoints
- simplification de la maintenance

Au cours de cette démonstration, j'ai répondu aux questions des participants et expliqué les choix techniques réalisés, notamment concernant l'interprétation des ClientException et la raison pour laquelle un composant dédié a été préféré à une modification du middleware existant.

Les échanges ont permis de valider la pertinence de la solution dans le contexte du BFF Assuré et de confirmer son intérêt pour de futures évolutions de l'application.

Cette présentation m'a permis de développer ma capacité à adapter mon discours à différents interlocuteurs techniques, à vulgariser des concepts d'architecture logicielle et à justifier des choix de conception réalisés dans un contexte professionnel.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.11/`) :

- `Pr#U00e9sentation du pattern Builder.pptx`

- `doc.md`


---


## Compétence 2.12

## Activité 2.12 : Validation de mise en exploitation

J'ai acquis cette compétence dans le cadre des évolutions réalisées sur le **BFF Assuré**.

Au sein de l'entreprise, les mises en production sont réalisées par les Team Leaders. En tant que développeur, je participe aux différentes étapes permettant de valider qu'une évolution est prête à être intégrée puis déployée dans l'environnement de production.

Avant toute mise en exploitation, les développements réalisés suivent un processus de validation comprenant plusieurs étapes :

- réalisation du développement
- revue de code via Merge Request
- correction des remarques éventuelles
- validation des tests techniques
- recette fonctionnelle
- validation documentaire
- préparation à l'intégration

L'un des exemples les plus représentatifs est l'évolution réalisée pour le projet **CARENET**, permettant la consultation des groupes de pays dans le module Nomenclatures de l'api NR2.

À l'issue du développement, la fonctionnalité a été déployée dans un environnement de recette afin d'être testée par les Business Analysts. Les différents scénarios prévus ont alors été vérifiés afin de confirmer la conformité du comportement développé avec le besoin exprimé.

Les résultats de cette recette ont ensuite été formalisés dans un **procès-verbal de recette**, signé par les parties prenantes. Cette validation constitue un prérequis nécessaire à la poursuite du processus d'intégration et de mise en exploitation.

Par ailleurs, après chaque mise en production, l'équipe réalise une phase de surveillance renforcée afin de vérifier le bon fonctionnement des applications en environnement réel.

Les mises en production étant généralement réalisées le mercredi matin, une partie de la journée est consacrée à l'analyse des journaux techniques centralisés dans **Elastic** et **Kibana**.

Cette surveillance permet notamment de contrôler :

- l'absence d'erreurs applicatives
- le bon fonctionnement des appels vers BPM et NR2
- l'absence d'augmentation anormale des erreurs HTTP
- la stabilité des nouvelles fonctionnalités déployées
- l'absence d'effets de bord ou de régressions

Lorsqu'une anomalie est détectée dans les journaux, les informations fournies par Elastic et Kibana permettent d'identifier rapidement son origine et de mettre en œuvre les actions correctives nécessaires.

Cette démarche contribue à sécuriser les mises en production et à garantir que les évolutions livrées répondent aux exigences de qualité attendues avant et après leur déploiement.

Cette activité m'a permis de comprendre les différentes étapes précédant une mise en exploitation, l'importance des validations réalisées en amont ainsi que les contrôles effectués après le déploiement afin de s'assurer du bon fonctionnement des applications en production.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.12/`) :

- `MergeRequest.png`

- `PV de recette_CHG0060931.docx`

- `kibana.png`

- `ticket.png`


---


## Compétences 2.13 & 2.14

## Activités 2.13 et 2.14 : Participation aux réunions, échanges avec les utilisateurs et communication avec les acteurs du projet

J'ai acquis ces compétences dans le cadre de mon alternance lors de la maintenance et de l'évolution du **BFF Assuré**.

Le développement des différentes fonctionnalités nécessitait une collaboration régulière entre les développeurs, les Team Leaders et les Business Analysts. Afin d'assurer une bonne compréhension des besoins exprimés et de coordonner les travaux de l'équipe, plusieurs réunions et échanges étaient organisés tout au long des projets.

J'ai notamment participé aux réunions quotidiennes ("daily meetings") réunissant les développeurs ainsi que les responsables techniques de l'équipe. Ces réunions avaient pour objectif de partager l'avancement des travaux, d'identifier les éventuels blocages rencontrés et de coordonner les actions à mener.

Au cours de ces échanges, je présentais régulièrement :

- les développements réalisés
- les tâches en cours
- les difficultés rencontrées
- les besoins d'accompagnement technique ou fonctionnel

Ces réunions me permettaient également de recueillir des informations complémentaires auprès des Business Analysts lorsque certaines règles de gestion nécessitaient d'être clarifiées avant le développement d'une fonctionnalité.

En complément de ces réunions, j'ai également participé à la rédaction et à la maintenance de la documentation technique associée aux évolutions réalisées.

Au sein de l'équipe, la documentation technique est systématiquement rédigée en français et en anglais afin de garantir sa réutilisation par l'ensemble des acteurs du projet et de conserver une homogénéité documentaire.

Dans ce contexte, j'ai contribué à la documentation de plusieurs fonctionnalités du BFF Assuré, notamment les endpoints liés aux nomenclatures de pays et de devises. Ces documents présentent :

- les points d'entrée exposés
- les paramètres d'entrée attendus
- les objets échangés
- les règles de gestion appliquées
- les interactions avec les services externes
- les erreurs pouvant être rencontrées

Lors de leur rédaction, j'ai veillé à respecter le format documentaire utilisé par l'équipe afin de garantir une présentation homogène et facilement exploitable par les développeurs, les Business Analysts ainsi que les futurs mainteneurs de l'application.

La production systématique d'une documentation bilingue m'a également conduit à adapter les formulations techniques au contexte anglophone tout en conservant la cohérence des contrats d'API, des structures JSON, des règles métier et des codes d'erreur utilisés dans l'application.

Cette participation aux échanges fonctionnels et techniques ainsi que la rédaction de documentation m'ont permis de développer ma capacité à :

- communiquer efficacement avec différents interlocuteurs
- adapter mon discours au public visé
- reformuler un besoin métier
- partager une information technique de manière structurée
- produire des documents exploitables en français et en anglais

Ces activités ont contribué à améliorer la circulation de l'information au sein de l'équipe et à garantir une compréhension commune des besoins, des développements réalisés et des solutions mises en œuvre.


**Preuves associées** (voir `preuves_completes.zip/bloc2/2.13_2.14/`) :

- `docEN.md`

- `docFR.md`

- `echangeBA.png`

- `pointDev.png`

- `pointDevBa.png`


---



# Bloc 3 — Préparer le déploiement d'une application logicielle sécurisée

\newpage


## Compétence 3.1

## Activité 3.1 : Définir les spécifications techniques et fonctionnelles de l’application numérique

J'ai acquis cette compétence dans le cadre d'une étude réalisée au sein de l'entreprise portant sur la mise en place de tests d'architecture automatisés pour les projets .NET.

L'objectif était de trouver une solution permettant de vérifier automatiquement le respect des règles d'architecture définies au sein du système d'information. Plusieurs contraintes restaient cependant à préciser avant de pouvoir démarrer le développement :

- comment détecter les dépendances interdites entre projets
- comment contrôler les conventions de développement
- à quel moment exécuter ces contrôles
- quelles technologies étaient les plus adaptées au contexte de l'entreprise

Afin de lever ces incertitudes, j'ai réalisé une étude comparative de plusieurs solutions techniques permettant d'effectuer des tests d'architecture. Cette étude a été formalisée dans une présentation destinée à mon tuteur, aux Team Leaders ainsi qu'à mon manager.

J'ai notamment analysé quatre solutions différentes :

- ArchUnitNET, bibliothèque permettant de réaliser des tests d'architecture sous forme de tests unitaires
- Roslyn Analyzer, extension du compilateur .NET permettant d'analyser le code en temps réel ou lors de la compilation
- Reflection, utilisant la réflexion .NET pour inspecter les assemblies
- NDepend, solution commerciale spécialisée dans l'analyse de l'architecture logicielle

Pour chacune de ces solutions, j'ai étudié :

- son fonctionnement ;
- les fonctionnalités proposées ;
- ses avantages ;
- ses limites ;
- sa capacité à répondre aux besoins de l'entreprise.

Cette analyse m'a permis de constater que certaines solutions répondaient uniquement à une partie du besoin. Par exemple, Reflection ne permettait pas une couverture suffisante des règles d'architecture tandis que NDepend introduisait une contrainte financière importante.

À l'issue de cette étude, la solution basée sur Roslyn Analyzer a été retenue en raison de plusieurs avantages :

- analyse du code directement dans l'environnement de développement
- contrôles exécutés lors de la compilation
- possibilité de personnaliser entièrement les règles
- possibilité d'ajouter des correctifs automatiques (CodeFix)
- intégration naturelle dans l'écosystème .NET

La présentation de ces résultats a permis de répondre aux interrogations techniques initiales et de valider collectivement la stratégie à adopter avant le démarrage du développement.

Cette démarche m'a permis d'apprendre à rechercher et comparer plusieurs solutions techniques, à formaliser leurs avantages et leurs limites, puis à présenter mes conclusions afin de faciliter la prise de décision


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.1/`) :

- `Tests d#U2019architecture.pptx`

- `exempleAnalyzer.md`

- `solution.png`


---


## Compétence 3.2

## Activité 3.2 : Développer à partir des spécifications fonctionnelles des algorithmes

J'ai acquis cette compétence lors du développement du **CatchErrorBuilder**, un composant destiné à standardiser la gestion des erreurs au sein du BFF Assuré.

Le besoin fonctionnel exprimé était de garantir qu'une erreur retournée par une API distante conserve son sens métier lorsqu'elle est propagée jusqu'au consommateur final. Dans notre architecture, les erreurs provenant d'APIs telles que NR2 ou BPM étaient encapsulées dans une `ClientException`. Cette encapsulation entraînait la perte de l'information métier portée par l'erreur initiale, les `ClientException` étant systématiquement interprétées comme des erreurs génériques renvoyant un code HTTP 500.

Afin de répondre à ce besoin, j'ai commencé par analyser le problème et le décomposer en plusieurs sous-problèmes :

- vérifier qu'un traitement métier a été défini ;
- exécuter ce traitement ;
- identifier la nature de l'exception rencontrée ;
- déterminer si un traitement spécifique a été configuré pour cette exception ;
- identifier l'erreur encapsulée lorsqu'il s'agit d'une `ClientException` ;
- déterminer le comportement à appliquer ;
- construire la réponse HTTP attendue.

Cette analyse m'a permis d'établir un algorithme de traitement structuré.

```text
Début

Vérifier qu'un traitement est défini

Si aucun traitement n'est défini
    Lever TraitementNullException

Exécuter le traitement

Si aucune exception n'est levée
    Retourner le résultat

Sinon

    Si l'exception hérite de BaseException

        Identifier son type métier

        Vérifier si un comportement spécifique existe

        Si oui
            Exécuter ce comportement

        Sinon
            Appliquer le traitement par défaut

    Sinon

        Retourner une erreur 500

Fin
```

Chaque étape de cet algorithme a ensuite été implémentée dans le composant. Par exemple, lorsqu'une API distante retourne une erreur de type DonneeNonTrouveeException, celle-ci est transformée par la couche cliente avant d'être propagée au BFF. Le CatchErrorBuilder n'a pas connaissance du type ClientException lui-même mais exploite les informations métier remontées par l'exception afin d'identifier son type et d'appliquer le comportement correspondant. Il est alors capable de restituer le code HTTP attendu au lieu de retourner systématiquement une erreur 500.

Cette approche permet également au développeur de définir des comportements personnalisés pour certaines erreurs :

```csharp
return await builder
    .DefinirTraitement(() => feature())
    .OnDonneeNonTrouve(_ => Results.NoContent())
    .Executer();
```

La décomposition préalable du problème m'a permis d'identifier clairement les différents cas à traiter avant même l'écriture du code. L'ensemble des règles fonctionnelles a ainsi pu être transcrit en un algorithme unique puis implémenté dans un composant réutilisable utilisé par les différents endpoints du BFF Assuré.

Cette démarche m'a permis de transformer une spécification fonctionnelle complexe en un algorithme structuré puis en une solution logicielle répondant aux besoins exprimés par le projet.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.2/`) :

- `Executer.md`

- `documentation.md`


---


## Compétence 3.3

## Activité 3.3 : Transcription des algorithmes en code source

J'ai acquis cette compétence lors du développement du système **RBAC** (Role Based Access Control) de l'entreprise.

L'objectif de cette évolution était de permettre la gestion centralisée des autorisations à partir de fichiers de configuration JSON. Le système devait être capable de construire dynamiquement les permissions attribuées à chaque partenaire tout en prenant en compte les relations d'héritage entre permissions.

La configuration du système repose sur plusieurs fichiers :

- un fichier décrivant la hiérarchie des permissions ;
- un fichier listant les partenaires existants ;
- un fichier de configuration propre à chaque partenaire définissant les permissions les plus élevées qui lui sont attribuées.

L'une des principales difficultés consistait à gérer automatiquement les relations de parenté entre permissions.

Par exemple :

```text
Permission C
 ├── Permission A
 │    └── Permission B
 └── Permission D
```

Dans cet exemple :

- la permission A donne automatiquement accès à la permission B ;
- la permission C donne automatiquement accès aux permissions A, B et D.

L'objectif était donc d'éviter la duplication des permissions dans les fichiers de configuration tout en garantissant que l'ensemble des droits hérités soient correctement restitués.

Afin de répondre à ce besoin, j'ai défini l'algorithme suivant :

```text
Charger les fichiers de configuration

Pour chaque partenaire

    Récupérer les permissions définies

    Pour chaque permission

        Ajouter la permission à la liste finale

        Rechercher les permissions enfants

        Pour chaque permission enfant

            Ajouter la permission enfant

            Rechercher récursivement ses enfants

    Supprimer les doublons

Retourner la liste finale des permissions
```

Une fois cet algorithme validé, j'ai procédé à sa transcription en code source C#.

L'implémentation réalisée permet de parcourir automatiquement l'arborescence des permissions définie dans les fichiers JSON, de construire l'ensemble des droits hérités et de produire la liste finale des autorisations réellement accordées à chaque partenaire.

Cette approche évite aux administrateurs de devoir déclarer manuellement toutes les permissions intermédiaires. Par exemple, lorsqu'un partenaire possède la permission **A** ainsi que la permission **B**, seule la permission **A** est conservée dans la configuration puisque la permission **B** est déjà héritée. En revanche, si un partenaire doit disposer des permissions **A** et **D** sans bénéficier de la permission **C**, alors les deux permissions sont explicitement renseignées et l'algorithme construit automatiquement les droits associés.

Afin de contrôler le résultat produit, j'ai également développé un endpoint permettant d'afficher l'ensemble des permissions calculées pour un partenaire donné. Cet endpoint facilite la vérification du bon fonctionnement de l'algorithme ainsi que la détection d'éventuelles erreurs de configuration.

Cette activité m'a permis de transformer des règles fonctionnelles complexes de gestion des autorisations en un algorithme structuré puis en une implémentation logicielle capable de construire dynamiquement les permissions utilisées par le système d'information.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.3/`) :

- `exemplePartenaire.json`

- `methodes.md`

- `structurePerm.json`


---


## Compétence 3.4

## Activité 3.4 : Modification d’un code existant et de son algorithme

J'ai acquis cette compétence lors de l'évolution du système de gestion des autorisations (**RBAC**) de l'entreprise.

Avant cette évolution, les permissions étaient directement définies dans le code source. Chaque profil utilisateur possédait une liste de permissions construite manuellement par les développeurs. Cette approche fonctionnait mais rendait l'ajout ou la modification des autorisations difficile à maintenir.

L'objectif du projet consistait à modifier ce fonctionnement afin d'externaliser la définition des permissions dans des fichiers de configuration JSON tout en conservant exactement le même comportement fonctionnel pour les applications utilisatrices.

Avant toute modification, j'ai dû analyser le fonctionnement existant afin de comprendre :

- comment les permissions étaient construites ;
- comment les relations entre permissions étaient définies ;
- comment les droits étaient attribués aux partenaires ;
- quelles contraintes de compatibilité devaient être respectées.

Une fois cette analyse réalisée, j'ai modifié l'algorithme de construction des permissions. Celui-ci ne reposait plus sur des structures codées en dur mais sur plusieurs fichiers de configuration :

- un fichier décrivant la hiérarchie des permissions ;
- un fichier listant les partenaires ;
- un fichier de configuration propre à chaque partenaire.

L'algorithme a été adapté afin de :

- charger dynamiquement les fichiers JSON ;
- reconstruire les relations entre permissions ;
- gérer l'effet d'héritage entre permissions ;
- éviter les doublons dans les autorisations ;
- restituer le même résultat que le système précédent.

Par exemple, lorsqu'une permission A autorise une permission B, il n'est plus nécessaire de déclarer explicitement la permission B pour un partenaire possédant déjà la permission A. L'algorithme reconstruit automatiquement les permissions héritées lors du chargement de la configuration.

Après la modification du code, plusieurs vérifications ont été réalisées afin de s'assurer que le nouveau système produisait les mêmes résultats fonctionnels que l'ancien. J'ai notamment développé un endpoint permettant de visualiser les permissions effectivement calculées pour chaque partenaire afin de comparer facilement les comportements obtenus.

Cette évolution m'a permis de modifier un algorithme existant en comprenant son fonctionnement initial, puis en l'adaptant à une nouvelle architecture tout en garantissant la compatibilité du résultat attendu.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.4/`) :

- `avant.md`

- `exemplePartenaire.json`

- `methodes.md`

- `structurePerm.json`


---


## Compétence 3.5

## Actvité 3.5 : Compilation, déverminage du code source

J'ai acquis cette compétence lors du développement d'une évolution du BFF Assuré.

Au cours des tests réalisés après l'implémentation d'une nouvelle fonctionnalité, j'ai constaté un comportement anormal lors des appels vers une API externe. Alors que les données attendues étaient correctement disponibles dans l'application cible, le BFF ne parvenait pas à récupérer la réponse attendue.

Afin d'identifier l'origine du problème, j'ai commencé par reproduire systématiquement l'anomalie dans mon environnement de développement. J'ai ensuite utilisé les outils de débogage de Visual Studio afin de suivre l'exécution du programme étape par étape et d'inspecter les variables manipulées pendant l'appel de l'API.

Cette analyse m'a permis de constater qu'une erreur de configuration était présente dans la définition du client utilisé pour communiquer avec le service distant. La configuration appliquée ne correspondait pas à celle attendue par l'API cible, ce qui empêchait le bon déroulement des échanges.

Après avoir identifié la cause du problème, j'ai corrigé la configuration concernée puis recompilé l'application.

Une nouvelle série de tests a ensuite été réalisée afin de vérifier :

- la disparition de l'anomalie observée$
- le bon fonctionnement de l'appel de service
- la récupération correcte des données attendue
- l'absence de régression sur les autres fonctionnalités utilisant le même mécanisme de communication

Les résultats obtenus ont confirmé la correction du problème ainsi que le bon fonctionnement de la fonctionnalité développée.

Cette activité m'a permis de mettre en œuvre une démarche complète de déverminage : reproduction d'une anomalie, analyse en mode Debug, identification de la cause racine, correction du code puis validation du comportement attendu au travers de tests techniques.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.5/`) :

- `Exception.png`

- `apresCorrection.png`

- `configApres.json`

- `configAvant.json`

- `variableLocales.png`


---


## Compétences 3.6 & 3.7

## Activité 3.6 et 3.7 : Agglomération des différents éléments logiciels en unités de traitement et intégration de fonctionnalités préprogrammées

J'ai acquis cette compétence lors du développement de fonctionnalités sur le BFF Assuré.

Le rôle du BFF (Backend For Frontend) est de masquer et simplifier le fonctionnement des APIs du système d'information, notamment NR2 et BPM, afin de fournir aux applications clientes une interface plus simple à utiliser.

Les APIs BPM et NR2 sont organisées sous la forme de monolithes modulaires. Elles regroupent plusieurs modules métier tels que Nomenclatures, Prestations Santé ou encore Contrats. Chaque module est composé de plusieurs projets, dont un projet particulier appelé **Client**.

À chaque livraison d'un module, le projet Client est publié sous la forme d'un package NuGet. Ce package permet aux autres applications de consommer facilement les fonctionnalités du module sans avoir à réimplémenter les mécanismes de communication.

Ces projets Clients encapsulent l'ensemble des éléments nécessaires à la communication avec leur module respectif :

- la configuration des appels HTTP ;
- les DTOs d'entrée ;
- les DTOs de sortie ;
- les interfaces de communication ;
- les implémentations des clients ;
- l'enregistrement des dépendances.

Par exemple, le module Nomenclatures met à disposition un package `Nomenclature.Client` contenant notamment l'interface `INomenclatureClient` ainsi que son implémentation `NomenclatureClient`. Les différents endpoints exposés par le module sont directement accessibles sous la forme de méthodes C#.

L'intégration d'une fonctionnalité dans le BFF consiste alors à assembler plusieurs composants déjà existants :

```text
Endpoint BFF
        ↓
Repository BFF
        ↓
NomenclatureClient
        ↓
API Nomenclatures
        ↓
Retour DTO
```

Cette approche permet de consommer directement les fonctionnalités métier exposées par les différents modules sans avoir à redévelopper :

- la gestion des appels HTTP
- la sérialisation des données
- les contrats d'échange
- la gestion des erreurs de communication

Le BFF s'appuie également sur différents Building Blocks mutualisés à l'échelle de l'entreprise. Ces composants regroupent des fonctionnalités transverses déjà développées et validées puis réutilisées dans plusieurs applications du système d'information, comme par exemple :

- gestion des erreurs
- gestion des résultats
- mécanismes de sécurité
- gestion du cache
- outils de journalisation et de diagnostic

Lors du développement d'une nouvelle fonctionnalité, ces composants sont directement intégrés au projet via l'injection de dépendances et utilisés conjointement aux projets Clients afin de constituer la chaîne complète de traitement d'une requête.

L'intégration de ces différents éléments permet de construire une unité de traitement complète en combinant des composants logiciels hétérogènes déjà existants.

Cette démarche présente également un intérêt en matière d'éco-conception logicielle. La mutualisation des composants permet de développer une fonctionnalité une seule fois puis de la réutiliser dans plusieurs applications. Cette approche limite la duplication du code, réduit les coûts de maintenance et évite la multiplication de développements équivalents au sein du système d'information.

L'ensemble de ces composants a été intégré afin de produire une application cohérente, prête à être soumise aux tests techniques puis déployée dans les différents environnements de l'entreprise.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.6_3.7/`) :

- `config.md`

- `exempleRepository.md`

- `projetClient.png`


---


## Compétence 3.8

## Activité 3.8 : Réalisation des tests unitaires

J'ai acquis cette compétence lors du développement du **CatchErrorBuilder**, un composant destiné à standardiser la gestion des erreurs au sein du BFF Assuré.

Compte tenu de son rôle central dans le traitement des exceptions, il était nécessaire de vérifier son bon fonctionnement dans l'ensemble des situations pouvant être rencontrées par les différents endpoints de l'application. J'ai donc mis en place plusieurs tests unitaires afin de valider les différents comportements attendus et prévenir l'apparition de régressions lors des évolutions futures.

Avant de développer les tests, j'ai identifié les principaux scénarios que devait couvrir le composant :

- exécution correcte d'un traitement sans erreur
- absence de traitement configuré
- traitement d'une exception métier
- traitement d'une exception métier possédant un comportement spécifique
- utilisation du comportement par défaut lorsqu'aucune règle spécifique n'est définie
- conversion des exceptions en `ProblemDetails`

J'ai ensuite construit différents jeux d'essai permettant de couvrir chacun de ces cas.

Par exemple, lorsqu'une exception de type `DonneeNonTrouveeException` est rencontrée, le test vérifie que le comportement configuré est correctement exécuté :

```csharp
builder
    .DefinirTraitement(...)
    .OnDonneeNonTrouve(_ => Results.NoContent())
    .Executer();
```

Le résultat attendu est alors une réponse HTTP `204 NoContent`.

D'autres scénarios vérifient qu'une exception sans traitement spécifique utilise correctement le comportement par défaut ou qu'une erreur technique produit une réponse cohérente avec les règles définies dans le composant.

L'exécution automatique de ces tests m'a permis de valider le bon fonctionnement du CatchErrorBuilder après chaque modification du code. Les anomalies détectées lors du développement ont ainsi pu être corrigées avant leur intégration dans le projet.

Cette démarche m'a également permis de vérifier l'absence de régression fonctionnelle lors de l'ajout de nouveaux comportements ou de nouvelles règles de gestion des erreurs.

L'ensemble des tests étant validés, les résultats obtenus ont confirmé que le composant produisait les réponses attendues dans tous les scénarios couverts et qu'aucun défaut d'exécution n'était détecté sur les cas de test définis.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.8/`) :

- `resultat.png`

- `tableau.md`

- `test.md`

- `testNonGere.md`


---


## Compétence 3.9

## Activité 3.9 : Mise à jour du planning de réalisation

J'ai acquis cette compétence dans le cadre de mon alternance au sein de l'entreprise.

Afin d'assurer le suivi des différentes missions qui me sont confiées, notre équipe utilise un canal Teams permettant de centraliser les activités réalisées, leur état d'avancement ainsi que le reste à faire estimé. Ces informations sont ensuite exploitées pour produire un compte-rendu hebdomadaire transmis à la hiérarchie.

Chaque semaine, je mettais à jour mon activité en indiquant :

- les sujets sur lesquels j'avais travaillé ;
- leur pourcentage d'avancement ;
- le nombre de jours restant estimé pour leur réalisation ;
- les éventuelles difficultés rencontrées ;
- les changements de priorité affectant mon planning.

Cette démarche permettait de suivre simultanément plusieurs projets et évolutions, qu'il s'agisse de développements sur le BFF Assuré, de travaux liés au RBAC, au CatchErrorBuilder ou d'autres sujets confiés au cours de mon alternance.

La mise à jour régulière de ces informations permettait d'identifier rapidement les écarts entre les estimations initiales et l'avancement réel des travaux. Lorsqu'une tâche nécessitait plus de temps que prévu ou qu'un blocage technique était rencontré, le reste à faire pouvait être réévalué afin de refléter plus fidèlement la situation du projet.

Ces informations étaient consultées par les Team Leaders, les chefs de projet ainsi que les responsables de l'équipe afin de disposer d'une vision globale de l'état d'avancement des différents sujets et d'anticiper les priorités des semaines suivantes.

Cette activité m'a permis de développer ma capacité à :

- estimer une charge de travail ;
- suivre l'avancement d'une tâche dans le temps ;
- identifier les écarts entre prévisionnel et réalisé ;
- réajuster mes estimations lorsque cela est nécessaire ;
- communiquer régulièrement sur mon activité.

La mise à jour de ce suivi hebdomadaire a ainsi contribué au pilotage des projets de l'équipe et à une meilleure visibilité de l'avancement des développements réalisés pendant mon alternance.


**Preuves associées** (voir `preuves_completes.zip/bloc3/3.9/`) :

- `NR2 Suivi avancement Semaine du 30042026.msg`

- `avancement13.04.png`

- `avancement27.04.png`


---



# Bloc 4 — Réaliser une interface d'échange de données informatisées

\newpage


## Compétence 4.1

## Activité 4.1 : Rétro-documentation de logiciels et de bases de données

J'ai acquis cette compétence lors de la réalisation de la documentation technique du **SQLLogRebuilder**, un outil interne utilisé par les développeurs de l'entreprise.

Cet outil permet de transformer les requêtes SQL présentes dans les logs générés par Entity Framework en requêtes SQL directement exploitables dans des outils d'analyse tels que DBeaver. Il facilite notamment les phases d'investigation et de diagnostic en permettant de reproduire localement les requêtes exécutées par les applications.

Le SQLLogRebuilder existait déjà avant mon intervention. Afin de produire une documentation fiable et exploitable, j'ai commencé par analyser son fonctionnement à partir de son code source et de son comportement lors de l'exécution.

Cette analyse m'a permis d'identifier :

- les formats de logs acceptés
- les données d'entrée utilisées par l'application
- les différents traitements effectués
- les données de sortie produites
- les cas d'utilisation principaux de l'outil

J'ai notamment constaté que l'application était capable de traiter des logs provenant de plusieurs sources :

- un fichier contenant des logs
- le contenu du presse-papier

ainsi que des logs de plusieurs formats :

- des logs au format Elastic/Kibana
- des logs récupérés depuis un terminal

Le fonctionnement général de l'outil peut être résumé par le processus suivant :

```text
Logs Entity Framework
        ↓
Analyse des paramètres SQL
        ↓
Récupération des valeurs associées
        ↓
Remplacement des paramètres
        ↓
Reconstruction de la requête SQL
        ↓
Script SQL exploitable dans DBeaver
```

Après avoir compris le fonctionnement du logiciel, j'ai rédigé une documentation présentant :

- l'objectif de l'outil
- son paramétrage
- les fichiers utilisés
- les différents modes d'utilisation
- les données d'entrée attendues
- les données produites en sortie

La documentation explique notamment les deux principaux modes de fonctionnement :

- reconstruction des requêtes depuis un fichier de logs ;
- reconstruction des requêtes à partir du contenu du presse-papier.

Elle permet aujourd'hui à un développeur de comprendre rapidement le rôle du SQLLogRebuilder et de l'utiliser sans avoir à analyser directement son implémentation.

Cette activité m'a permis de développer ma capacité à étudier un logiciel existant, à comprendre son fonctionnement interne puis à formaliser ces connaissances sous forme d'une documentation technique fiable, utilisable et maintenable par les membres de l'équipe.


**Preuves associées** (voir `preuves_completes.zip/bloc4/4.1/`) :

- `_static/image.png`

- `documentation.md`

- `exemple.md`


---


## Compétence 4.2

## Activité 4.2 : Mise au point de tables de correspondance de données

J'ai acquis cette compétence lors du développement de la fonctionnalité **ListerDecomptesEdiGeles** du BFF Assuré.

Le rôle du BFF est de simplifier les échanges entre les applications clientes et les APIs du système d'information. Pour cela, il doit transformer les objets retournés par les APIs métiers afin de les adapter aux besoins des applications consommatrices.

Dans le cadre de cette fonctionnalité, le module **Décomptes EDI Gelés** du BPM retourne un ensemble de DTOs qui ne correspondent pas directement au contrat exposé par le BFF. J'ai donc mis en place un composant de mapping chargé d'assurer la correspondance entre les structures de données des deux applications.

Le flux de transformation est le suivant :

```text
API BPM

ListerDecomptesEdiGelesAssureOutput

                ↓

ListerDecomptesEdiGelesMapper

                ↓

ListerDecomptesEdiGelesOutput

                ↓

Application cliente
```

Le mapper centralise l'ensemble des règles de correspondance entre les objets BPM et les objets du BFF.

Par exemple, les informations du bénéficiaire sont transformées selon les règles suivantes :

| DTO BPM                       | DTO BFF                       |
| ----------------------------- | ----------------------------- |
| Identifiant                   | Identifiant                   |
| Nom                           | Nom                           |
| Prenom                        | Prenom                        |
| NumeroNationalImmatriculation | NumeroNationalImmatriculation |

De la même manière, les montants sont convertis vers le contrat exposé par le BFF :

| DTO BPM | DTO BFF |
| ------- | ------- |
| Valeur  | Montant |
| Devise  | Devise  |

Une correspondance particulière a également été mise en place pour l'identifiant du décompte. Le BPM retourne un identifiant unique contenant plusieurs informations métier :

```text
125-78-456
```

Le BFF doit exposer ces informations dans trois propriétés distinctes :

| Donnée source BPM      | Donnée cible BFF           |
| ---------------------- | -------------------------- |
| IdentifiantDecompteEdi | IdentifiantFichier         |
| IdentifiantDecompteEdi | IdentifiantLot             |
| IdentifiantDecompteEdi | IdentifiantDecompteArchive |

Cette transformation est réalisée par la méthode :

```csharp
SplitIndentifiantDecompte()
```

Le composant de mapping garantit ainsi que chaque donnée manipulée par le BPM possède une correspondance clairement définie dans le modèle exposé par le BFF.

Cette approche facilite la maintenance du système tout en garantissant qu'aucune donnée nécessaire au fonctionnement de l'application cliente ne soit perdue lors des échanges entre les différentes applications.

Cette activité m'a permis d'analyser les modèles de données de deux applications distinctes puis de formaliser les règles de correspondance nécessaires à leurs échanges.


**Preuves associées** (voir `preuves_completes.zip/bloc4/4.2/`) :

- `mapper.md`

- `mappingDto.md`


---


## Compétence 4.3

## Activité 4.3 : Consolidation, agrégation de données

J'ai acquis cette compétence lors du développement de la fonctionnalité **ListerDecomptesEdiGeles** du BFF Assuré.

Dans cette fonctionnalité, certaines informations attendues par les applications clientes n'étaient pas directement disponibles dans les données retournées par le BPM. Il a donc été nécessaire de produire de nouvelles données à partir de plusieurs informations existantes.

L'exemple le plus représentatif est la génération du champ :

```text
LibelleDecompteArchive
```

Cette donnée n'est jamais retournée directement par le BPM.

Pour la construire, le BFF applique la règle métier PRS-DEC-007 qui agrège des données provenant de plusieurs services du système d'information.

Les décomptes sont récupérés auprès du module Prestations Santé du BPM tandis que les nomenclatures nécessaires à l'application de la règle métier sont récupérées au travers d'un appel distinct vers le module Nomenclatures de NR2.

Ces différentes données sont ensuite consolidées afin de produire le libellé du décompte attendu par l'application cliente.

Le mécanisme de consolidation peut être représenté ainsi :

```text
Partenaire exécutant
           │
           ├── Spécialité
           ├── Nomenclatures Pharmacie
           ├── Nomenclatures Biologie
           ├── Nomenclatures Dentaire
           ├── Nomenclatures Consultation
           └── Lignes du décompte

                    ↓

         TrouverLibelleDecompte()

                    ↓

        LibelleDecompteArchive
```

La méthode applique la règle PRS-DEC-007 suivante :

1. Vérifier si la spécialité du partenaire appartient à une nomenclature appartenant au groupe Pharmacie, Biologie ou Dentaire.
2. Si une correspondance existe, utiliser le libellé de cette spécialité.
3. Sinon, analyser les actes présents dans les lignes du décompte.
4. Rechercher un acte de consultation.
5. Utiliser le premier acte correspondant trouvé.
6. À défaut, utiliser le premier acte disponible.

Cette logique permet de calculer automatiquement une donnée métier inexistante dans les données source.

Le schéma global de consolidation est le suivant :

```text
BPM
│
└── DecompteEdiGeles
       │
       ├── PartenaireExecutant
       └── LignesDecompteEdi

NR2
│
├── Nomenclatures Spécialité partenaire
│   ├── Groupe Pharmacie
│   ├── Groupe Biologie
│   └── Groupe Dentaire
└── Nomenclatures Code Acte
    └── Libellés contenant "Consultation"

                ↓

          PRS-DEC-007

                ↓

       LibelleDecompteArchive

                ↓

          DTO BFF

                ↓

       Application cliente
```

Ainsi, alors que le BPM ne retourne que des informations techniques liées au partenaire exécutant et aux actes réalisés, le BFF construit une nouvelle donnée métier directement exploitable par le front. Cette donnée est obtenue sans saisie supplémentaire et repose uniquement sur l'agrégation des informations provenant du BPM et de NR2.

Cette approche permet de produire des informations directement exploitables par le front sans lui imposer la connaissance des règles métier ou des structures internes du BPM.

Cette activité m'a permis de mettre en œuvre un mécanisme d'agrégation de données provenant de plusieurs APIs du système d'information. Les informations retournées par le BPM ont été enrichies à l'aide de données récupérées dans NR2 afin de produire une nouvelle information métier, directement exploitable par les applications clientes, sans leur imposer la connaissance des règles de consolidation sous-jacentes.


**Preuves associées** (voir `preuves_completes.zip/bloc4/4.3/`) :

- `SplitIdentifiant.md`

- `TrouverLibelle.md`

- `appelsClient.md`

- `mappingDto.md`


---


## Compétence 4.4

## Activité 4.4 : Contrôler les flux de données entre les logiciels

J'ai acquis cette compétence lors du développement de plusieurs fonctionnalités du **BFF Assuré**.

Le rôle du BFF (Backend For Frontend) est de servir d'intermédiaire entre les applications clientes et les différents services du système d'information. Il permet ainsi d'assurer les échanges de données entre plusieurs logiciels tout en masquant leur complexité aux consommateurs.

Dans l'architecture mise en place, le BFF communique principalement avec les APIs BPM et NR2 au travers des projets Clients mis à disposition par chaque module métier.

Le flux de données peut être représenté de la manière suivante :

```text
Application cliente
        │
        ▼

   BFF Assuré
        │
        ▼

  Projet Client
        │
        ▼

     API BPM
        │
        ▼

     API NR2
        │
        ▼

  Base de données
```

Lorsqu'une fonctionnalité est exécutée, le BFF importe les données provenant des APIs métier puis les transforme afin de les adapter aux besoins du front avant de les réexporter sous un format simplifié.

Par exemple, pour la fonctionnalité de consultation des décomptes EDI gelés :

```text
      Front
        │
        ▼

    BFF Assuré
        │
        ▼

Client Prestations Santé
        │
        ▼

    API BPM
        │
        ▼

    DTO BPM
        │
        ▼

  Transformation BFF
        │
        ▼

     DTO BFF
        │
        ▼

      Front
```

Les échanges entre les différentes applications reposent sur des DTOs communs transportés au format JSON. L'utilisation des projets Clients permet de garantir la compatibilité entre les systèmes émetteurs et récepteurs tout en limitant les risques d'erreur liés à la sérialisation ou à l'évolution des contrats d'échange.

Afin de faciliter le contrôle des flux de données, chaque client encapsule :

- les points d'entrée de l'API distante
- les DTOs d'entrée
- les DTOs de sortie
- la configuration HTTP
- les mécanismes de sérialisation
- la gestion des erreurs technique

Cette approche permet de mesurer et contrôler les flux de données échangés entre le BFF et les différents services métier. Elle garantit également que les données transmises restent cohérentes tout au long de la chaîne de traitement.

L'ensemble des fonctionnalités développées repose ainsi sur des flux synchrones maîtrisés permettant l'importation, la transformation puis l'exportation des données entre plusieurs logiciels du système d'information.

Cette activité m'a permis d'acquérir une compréhension approfondie des mécanismes d'échange de données entre applications et des contraintes liées à la compatibilité des systèmes communiquant entre eux.


**Preuves associées** (voir `preuves_completes.zip/bloc4/4.4/`) :

- `configClient.md`

- `exempleLog.png`

- `schema.drawio`


---


## Compétence 4.5

## Activité 4.5 : Réalisation d’un environnement de tests

J'ai acquis cette compétence lors du développement du projet **ITeralis**, une plateforme communautaire permettant la consultation et le partage de cours de cybersécurité.

Afin de faciliter le développement, les tests et le déploiement de l'application, j'ai mis en place un environnement complet permettant de reproduire localement l'architecture d'exécution de la solution.

L'application repose sur une architecture multi-tiers composée de plusieurs composants :

- une base de données MariaDB ;
- une API .NET 8 ;
- une application Web développée avec Deno/Fresh.

Afin d'automatiser l'installation et la configuration de ces différents éléments, j'ai utilisé **Docker** et **Docker Compose**.

Le fichier `docker-compose.yml` permet de créer automatiquement l'ensemble des composants nécessaires à l'exécution de l'application :

```text
Frontend (Deno/Fresh)
          │
          ▼
      API .NET
          │
          ▼
       MariaDB
```

L'architecture est organisée autour de trois conteneurs :

### Conteneur base de données

Le conteneur `bdd` repose sur une image MariaDB et initialise automatiquement la base grâce à un script SQL exécuté au démarrage.

Il assure :

- le stockage des données ;
- l'exécution du script d'initialisation ;
- la persistance des données via un volume Docker ;
- la vérification automatique de disponibilité grâce à un HealthCheck.

### Conteneur API

Le conteneur `api` est généré automatiquement à partir du code source de l'application.

Le Dockerfile réalise plusieurs étapes :

```text
Restauration des dépendances
        ↓
Compilation
        ↓
Publication .NET
        ↓
Création de l'image finale
```

Cette approche garantit que la version exécutée dans l'environnement de test est identique à celle qui sera déployée ultérieurement.

### Conteneur Frontend

Le conteneur `front` est construit à partir d'une image Deno.

Lors du build :

```text
Copie du code source
        ↓
Installation des dépendances
        ↓
Compilation de l'application
        ↓
Démarrage du serveur Web
```

Le front peut ainsi communiquer directement avec l'API au travers du réseau Docker prévu à cet effet.

### Isolation des flux réseau

Afin de reproduire un environnement proche de la production, deux réseaux Docker distincts ont été mis en place :

```text
iteralis-frontend

Front
    ↓
API
```

```text
iteralis-backend

API
    ↓
MariaDB
```

Cette séparation empêche l'application web d'accéder directement à la base de données et garantit que l'ensemble des échanges passe obligatoirement par l'API.

### Automatisation de l'environnement

Le démarrage de l'ensemble de la plateforme est réalisé par une seule commande :

```bash
docker compose up
```

Cette commande :

- construit les images ;
- crée les conteneurs ;
- crée les réseaux ;
- démarre MariaDB ;
- démarre l'API ;
- démarre l'application Web ;
- configure automatiquement les communications entre les services.

L'arrêt complet de l'environnement s'effectue simplement avec :

```bash
docker compose down
```

### Schéma de l'environnement de tests

```text
┌─────────────────────┐
│ Frontend Deno/Fresh │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│      API .NET 8     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│      MariaDB 11     │
└─────────────────────┘
```

La mise en place de cet environnement m'a permis de disposer d'une plateforme complète de développement et de validation, fidèle à l'architecture réelle de l'application. L'ensemble des composants peut être déployé de façon reproductible sur n'importe quelle machine disposant de Docker, ce qui facilite les tests techniques et garantit l'homogénéité des environnements utilisés par les différents développeurs du projet.


**Preuves associées** (voir `preuves_completes.zip/bloc4/4.5/`) :

- `docker-compose.yml`

- `dockerFiles.md`

- `init.sh`


---
