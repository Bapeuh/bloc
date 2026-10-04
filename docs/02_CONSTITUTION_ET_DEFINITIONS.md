# Constitution et définitions de BLOC

Ce document formalise le vocabulaire et les règles directrices du projet.

Il conserve l'hypothèse actuelle : **le Bloc fondamental ne possède pas de contenu intrinsèque**. Il possède une identité ; contenu, propriété, type et état doivent émerger des relations et de l'interprétation.

Une correction importante est désormais explicitement admise :

> **La simplicité de l'ontologie BLOC ne doit pas être confondue avec la simplicité de la machine qui l'exécute.**

# I. Axiomes ontologiques

## Axiome 1 — Identité

Un Bloc fondamental est une identité distinguable des autres.

## Axiome 2 — Minimalité sémantique

Un Bloc fondamental ne possède pas intrinsèquement de contenu, de type, de propriété métier ou de signification.

## Axiome 3 — Composition relationnelle

Les structures complexes émergent de relations entre Blocs.

## Axiome 4 — Interprétation

Le sens, le type, le rôle et l'état sont des interprétations de structures dans un contexte donné.

## Axiome 5 — Multiplicité des interprétations

Une même structure peut être interprétée par plusieurs lentilles sans changer d'identité logique.

## Axiome 6 — Transformation explicite

Une évolution ne doit pas réécrire silencieusement l'histoire logique. Les transformations, dérivations, résolutions et révocations doivent pouvoir être représentées explicitement.

## Axiome 7 — Causalité

La dépendance causale est plus fondamentale que l'ordre chronologique total.

## Axiome 8 — Séparation logique / physique

Une identité logique est distincte de ses matérialisations physiques.

# II. Principes d'architecture

Ces règles guident la conception mais ne sont pas des axiomes ontologiques.

## Principe A — Minimalité des primitives

Toute primitive proposée doit être justifiée par l'impossibilité raisonnable de la faire émerger des primitives existantes.

## Principe B — Coût réel de la Machine

Tout mécanisme que le système doit garantir réellement doit être compté dans la BLOC Machine, même s'il n'appartient pas à l'ontologie.

## Principe C — Représenter n'est pas garantir

Le fait qu'un conflit, une permission, un effet ou une causalité puisse être représenté dans le graphe ne signifie pas que la Machine garantit automatiquement sa résolution correcte.

## Principe D — Coordination seulement quand nécessaire

BLOC ne cherche pas à supprimer toute coordination. Il cherche à éviter de l'imposer lorsque la sémantique du calcul ne l'exige pas.

Les invariants globaux, décisions exclusives et effets externes peuvent nécessiter sérialisation, consensus, autorité unique ou autre coordination explicite.

## Principe E — Frontière physique explicite

Le monde physique n'est pas append-only et ne suit pas automatiquement les mêmes propriétés que le graphe logique.

Toute interaction physique doit distinguer au moins :

~~~text
intention
↓
autorisation
↓
tentative d'effet
↓
effet observé / preuve / résultat inconnu
~~~

## Principe F — Contrôle d'accès avant observation

Une politique de confidentialité appliquée après lecture est trop tardive.

L'accès à une structure protégée doit être médié avant que l'information ne soit rendue observable.

# III. Définitions

## Bloc

Identité logique fondamentale.

## Relation

Structure associant des Blocs. Une relation sémantiquement riche doit elle-même pouvoir être représentée dans l'univers BLOC.

## Univers BLOC

Ensemble logique des Blocs, relations, transformations et dépendances causales considérés dans un domaine.

## Core Model

Définition sémantique de l'univers BLOC : identité, relations, transformations, causalité et règles logiques associées.

Le Core Model n'est pas la machine physique.

## Rule Engine

Composant chargé d'évaluer un langage de règles limité et de proposer ou produire des transformations.

MATCH → PRODUCE est une abstraction de ce composant, pas encore une instruction universelle suffisamment spécifiée.

## BLOC Machine

Runtime de confiance chargé de matérialiser et protéger le modèle.

Elle peut devoir fournir des garanties que l'ontologie ne contient pas : atomicité, médiation d'accès, isolation, primitives de concurrence, interaction hardware, etc.

## Seed

Sous-graphe initial chargé au démarrage.

Le Seed n'est ni une primitive ontologique ni la BLOC Machine.

## Trust Anchor

Mécanisme physique ou cryptographique servant de fondement à certaines identités, preuves ou décisions d'autorité.

## Médiateur d'accès

Composant de la Trusted Computing Base qui décide si une observation ou une action protégée peut être effectuée avant de révéler ou modifier l'information.

## Lentille

Ensemble de règles d'interprétation appliquées à une partie de l'univers BLOC.

## Vue

Projection perceptible ou manipulable produite à partir d'une interprétation.

## Agent

Bloc interprété comme capable d'observer, décider, proposer ou produire des transformations.

## Événement / transformation

Structure représentant une évolution, sa provenance et ses dépendances causales lorsque celles-ci sont connues.

## Contexte

Sous-univers pertinent ou accessible pour une observation ou une action donnée.

## Capacité d'autorité

Preuve ou droit non forgeable permettant certaines observations ou actions.

Le terme « capacité » ne doit plus servir à désigner indistinctement des ressources matérielles ou des conditions de disponibilité.

## Condition de disponibilité

Condition nécessaire à l'activation d'un service, d'une vue ou d'un contexte.

## Ressource physique

CPU, mémoire, stockage, périphérique ou autre mécanisme matériel fini.

## Provenance

Structure causale permettant de déterminer l'origine et les dépendances d'une information ou transformation.

La provenance peut elle-même être sensible.

## Matérialisation

Représentation physique d'une identité ou structure logique.

Plusieurs matérialisations peuvent représenter la même identité logique.

## Dérivation pure

Transformation dont le résultat dépend uniquement d'entrées explicitement identifiées et d'une règle déterministe ou suffisamment reproductible.

## Observation externe

Information provenant de SENSE, d'un réseau, d'une horloge, d'un générateur non déterministe ou de tout autre élément non reproductible à partir du seul graphe logique.

# IV. Points de vigilance désormais établis

## 1. MATCH n'est pas arbitraire

Un matching général de sous-graphe n'est pas une primitive acceptable sans restriction.

Le langage de motifs devra être borné, local, indexable ou autrement limité afin que coût, terminaison et déterminisme puissent être raisonnés.

## 2. Append-only n'implique pas monotonicité totale

Les révocations, tests d'absence, consommations uniques et invariants globaux introduisent des comportements non monotones.

Ces cas doivent être explicitement identifiés.

## 3. Représentation de conflit ≠ résolution de conflit

Le graphe peut conserver plusieurs branches concurrentes. Cela ne suffit pas pour garantir des invariants globaux ou empêcher deux effets physiques incompatibles.

## 4. ACTUATE n'est pas exactly-once par défaut

Une tentative d'effet externe peut produire :

~~~text
confirmé
échoué
inconnu
~~~

L'idempotence, la déduplication, la compensation ou une autorité de sérialisation peuvent être nécessaires.

## 5. Révocation

L'historique append-only facilite l'audit d'une révocation, pas son enforcement.

La validité actuelle d'une autorité doit être vérifiée au moment de l'usage.

## 6. Reconstructibilité

Une donnée n'est reconstructible que si toutes ses dépendances nécessaires restent disponibles et si sa dérivation est suffisamment pure et déterministe.

Les observations externes ne sont pas automatiquement reconstructibles.

## 7. Oubli

Oublier une source peut nécessiter d'invalider ou d'effacer ses dérivés.

La provenance peut servir à suivre la contamination informationnelle, mais elle peut aussi constituer une fuite et doit être protégée en conséquence.

# V. Distinctions à préserver

Ne pas confondre :

- ontologie minimale et machine minimale ;
- Core Model et BLOC Machine ;
- Rule Engine et BLOC Machine ;
- Seed et Machine ;
- capacité d'autorité et ressource physique ;
- condition de disponibilité et capacité d'autorité ;
- identité logique et adresse physique ;
- existence logique et matérialisation ;
- observation et interprétation ;
- représentation d'un conflit et résolution d'un invariant ;
- intention et effet physique ;
- causalité et chronologie ;
- dérivation pure et observation externe ;
- historique logique et conservation éternelle de tous les octets.

# VI. Questions encore ouvertes

- encodage concret des identités ;
- modèle minimal de lien ;
- langage exact du Rule Engine ;
- restrictions exactes de MATCH ;
- identité déterministe d'une application de règle ;
- conditions d'idempotence ;
- atomicité minimale garantie ;
- stratégie de concurrence et d'ordonnancement ;
- opérations nécessitant coordination ;
- modèle de révocation ;
- modèle de Trust Anchor ;
- frontières du Médiateur d'accès ;
- fast paths hardware hors graphe ;
- statut des interruptions, MMU, timers et scheduling ;
- traitement des effets externes inconnus ;
- compaction et oubli transitif ;
- confidentialité de la provenance ;
- identité et réplication entre machines ;
- preuve d'expressivité du Seed ;
- performances et indexation.

Ces questions sont maintenant regroupées dans 04_BLOC_MACHINE_SPEC.md.
