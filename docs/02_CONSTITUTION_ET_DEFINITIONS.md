# Constitution et définitions de BLOC

Ce document formalise le vocabulaire et les règles directrices du projet.

Il remplace un point important des formulations historiques : **le contenu n'est plus considéré comme une propriété intrinsèque optionnelle du Bloc fondamental**. L'hypothèse actuelle est plus radicale : un Bloc fondamental possède une identité ; contenu, propriété, type et état doivent émerger des relations et de l'interprétation.

Cette évolution est la base de travail actuelle et doit continuer à être éprouvée.

# I. Constitution

## Article 1 — Universalité

Tout élément représenté dans l'univers BLOC doit pouvoir être représenté par des Blocs.

## Article 2 — Identité

Un Bloc possède une identité unique et stable dans le domaine où cette identité est reconnue.

## Article 3 — Minimalité

Un Bloc fondamental ne possède pas intrinsèquement de contenu, de type, de propriété métier ou de signification.

Il est d'abord une identité distinguable des autres.

## Article 4 — Existence indépendante

Un Bloc peut exister sans devoir appartenir à un fichier, un document, un processus, une application ou toute autre catégorie supérieure.

## Article 5 — Composition relationnelle

Les structures complexes émergent de relations entre Blocs.

## Article 6 — Symétrie ontologique

Une relation riche doit elle-même pouvoir être représentée comme un Bloc et participer à d'autres relations.

Les détails physiques d'encodage peuvent être optimisés ; ils ne doivent pas être confondus avec l'ontologie logique.

## Article 7 — Émergence du sens

Le sens n'est pas une propriété intrinsèque du Bloc. Il émerge de la structure, du contexte d'observation et de l'interprétation.

## Article 8 — Émergence du type

Le type est une interprétation. « Document », « utilisateur », « message », « application », « agent » ou « fichier » ne sont pas présumés fondamentaux.

## Article 9 — Multiplicité des interprétations

Une même structure peut être observée par plusieurs lentilles et produire plusieurs interprétations ou vues.

## Article 10 — Évolution explicite

Une évolution de l'univers ne doit pas être une réécriture silencieuse de son histoire.

L'hypothèse privilégiée est une évolution causale et largement append-only : les nouvelles structures expriment les transformations, dérivations, révocations, résolutions et changements d'état.

## Article 11 — Histoire native

La provenance et les relations causales font partie de la réalité représentable du système.

L'état courant peut être une interprétation de cette histoire plutôt qu'une valeur écrasée.

## Article 12 — Temps causal

Le temps logique fondamental est d'abord causal : un événement peut dépendre d'un autre sans qu'un ordre total soit nécessaire.

Le temps chronologique peut être apporté par une horloge et interprété comme information supplémentaire.

## Article 13 — Concurrence émergente

Deux transformations sans dépendance causale peuvent être considérées comme concurrentes, même si le matériel les exécute séquentiellement.

## Article 14 — Conflit représentable

Deux branches incompatibles du point de vue d'une interprétation peuvent coexister dans le graphe.

La résolution du conflit peut elle-même être une transformation explicite.

## Article 15 — Permissions émergentes

Les permissions, capacités, délégations et révocations doivent autant que possible être représentées par des Blocs, des relations et des preuves structurelles.

## Article 16 — Observation contextuelle

Un agent ne doit pas nécessairement observer l'univers entier. Son contexte et ses capacités déterminent ce qui lui est accessible et transformable.

## Article 17 — Agentivité émergente

Un agent est un Bloc interprété comme capable d'observer et/ou d'agir sur l'univers.

Humain, IA, service, capteur ou périphérique ne sont pas des natures fondamentales différentes.

## Article 18 — Neutralité

Le substrat BLOC ne privilégie aucun domaine d'usage.

## Article 19 — Séparation logique / physique

L'ontologie BLOC et sa matérialisation physique sont distinctes.

Une implémentation peut employer RAM, SSD, blobs, index, caches, tables, fichiers ou réseaux sans transformer ces mécanismes en primitives ontologiques.

## Article 20 — Test de nécessité

Toute nouvelle primitive proposée au cœur doit répondre à la question :

> Peut-elle réellement émerger des primitives existantes ?

Si oui, elle doit rester au-dessus du cœur.

# II. Définitions

## Bloc

Identité fondamentale distinguable des autres identités.

Dans l'hypothèse actuelle, le Bloc fondamental ne contient pas intrinsèquement de valeur ou de type.

## Relation

Structure permettant d'associer des Blocs. Une relation sémantiquement riche est elle-même représentable dans BLOC.

## Univers BLOC

Ensemble logique des Blocs, relations et structures causales considérés dans un domaine d'observation.

## Lentille

Ensemble de règles permettant d'interpréter une partie de l'univers BLOC.

Exemples de familles : relationnelles, documentaires, calculatoires, temporelles, spatiales, cognitives.

## Vue

Projection perceptible ou manipulable d'une interprétation produite par une lentille.

## Agent

Bloc interprété comme capable d'observer, décider, proposer ou produire des transformations.

## Événement / transformation

Structure représentant qu'une évolution a eu lieu, avec sa provenance et ses dépendances lorsque celles-ci sont connues.

## Contexte

Sous-univers accessible ou pertinent pour une observation ou une action donnée.

## Capacité

Structure servant de preuve ou de condition d'accès à une action, une observation ou une ressource.

## Provenance

Chaîne causale permettant de déterminer d'où vient une structure, une demande ou une transformation.

## Matérialisation

Représentation physique actuelle d'une identité ou structure logique : RAM, stockage persistant, réseau, blob compact, etc.

Plusieurs matérialisations peuvent représenter la même identité logique.

## Lentille de compatibilité

Lentille projetant BLOC vers une abstraction extérieure existante : filesystem POSIX, table SQL, API, document, etc.

# III. Distinctions à préserver

Ne pas confondre :

- identité logique et adresse physique ;
- existence logique et matérialisation ;
- Bloc et représentation binaire du Bloc ;
- lentille et vue ;
- observation et interprétation ;
- signal physique et intention ;
- causalité et chronologie ;
- politique et mécanisme matériel ;
- histoire logique et conservation éternelle de tous les octets.

# IV. Questions encore ouvertes

Ces points ne sont pas encore figés :

- encodage concret d'une identité BLOC ;
- représentation minimale des liens dans la machine primitive ;
- convention bootstrap exacte de `MATCH → PRODUCE` ;
- déterminisme et stratégie d'évaluation ;
- preuve d'expressivité du Seed ;
- règles de compaction et d'oubli logique ;
- identité et réplication entre machines ;
- racine de confiance et modèle cryptographique ;
- performances et structures d'indexation ;
- frontière exacte entre BLOC Machine et univers BLOC.

Ces questions doivent être résolues par expérimentation sans compromettre le principe de minimalité.
