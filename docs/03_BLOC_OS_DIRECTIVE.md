# Directive d'architecture — BLOC OS

## 0. Objet

Ce document synthétise l'exploration conceptuelle de BLOC appliquée à la construction d'un système d'exploitation.

Il ne constitue pas encore une spécification d'implémentation. Il définit une direction et des hypothèses à éprouver.

L'objectif est volontairement radical :

> **Imaginer BLOC OS avant de demander comment Linux pourrait l'implémenter.**

Linux, Docker, un hyperviseur ou un environnement utilisateur pourront servir de banc d'essai. Ils ne doivent pas dicter l'ontologie du système.

---

# 1. Point de départ

Un système classique expose comme catégories fondamentales :

```text
fichiers
répertoires
processus
threads
utilisateurs
permissions
sockets
services
applications
périphériques
```

BLOC OS cherche à ne présumer aucune de ces catégories.

Le cœur conceptuel part de :

```text
IDENTITÉS
+
RELATIONS
```

et tente de faire émerger le reste.

Le critère de réussite n'est pas d'avoir moins de code à tout prix. Il est de trouver **le minimum ontologique nécessaire**.

# 2. Atome fondamental

Hypothèse de travail :

> **Un Bloc fondamental est une identité sans contenu intrinsèque.**

Ne pas modéliser primitivement :

```text
Bloc {
    id
    type
    value
    properties
}
```

mais tendre conceptuellement vers :

```text
Bloc = identité
```

Valeur, type, propriété et contenu doivent être représentés par des structures relationnelles et interprétés par des lentilles.

# 3. Relations

Une relation sémantique doit pouvoir être un Bloc.

Le Core ne doit pas connaître nativement des relations métier telles que `possède`, `contient`, `source` ou `destination`.

L'orientation, les rôles et l'ordre doivent autant que possible émerger de structures.

Ce principe permet de représenter valeurs, relations orientées, séquences, transformations et règles sans multiplier les primitives.

# 4. Exécution

Un graphe statique peut représenter beaucoup de choses, mais il ne vit pas.

La candidate minimale d'exécution est :

> **MATCH → PRODUCE**

```text
structure existante
       ↓
      MATCH
       ↓
règle applicable
       ↓
     PRODUCE
       ↓
nouveaux Blocs + nouveaux liens
```

L'exécution devient :

> **un graphe qui produit du graphe.**

Code, données, règles et historique restent alors représentables dans la même ontologie.

# 5. BLOC Machine

La couche physique minimale est provisoirement appelée **BLOC Machine**.

Hypothèse actuelle :

```text
BLOC MACHINE 0

substrat :
    identité
    lien

capacités :
    créer
    relier
    observer

mécanisme :
    MATCH → PRODUCE

frontière physique :
    SENSE / ACTUATE
```

La différence entre primitive ontologique et capacité d'implémentation doit rester explicite.

# 6. Évolution et histoire

BLOC privilégie une évolution où une transformation ne réécrit pas silencieusement le passé.

```text
A
│
├── transformation T1 → B
└── transformation T2 → C
```

A, B, C et les transformations peuvent coexister.

L'état courant devient une interprétation du graphe causal.

Conséquences :

- historique natif ;
- provenance native ;
- versions émergentes ;
- branches représentables ;
- résolution explicite des conflits.

Cela ne signifie pas que chaque octet doit être conservé éternellement.

# 7. Temps et concurrence

Le temps logique fondamental est causal.

```text
A
├── E1 → B ─┐
└── E2 → C ─┴→ E3
```

E1 et E2 n'ont pas besoin d'un ordre logique total.

Le matériel peut les exécuter séquentiellement sans imposer cet ordre au modèle.

> **La concurrence émerge de l'absence de dépendance causale.**

Le temps chronologique est une observation supplémentaire fournie par une horloge.

# 8. Conflits

BLOC doit d'abord pouvoir représenter :

```text
    A
   / \
  B   C
```

puis éventuellement :

```text
B + C → D
```

La résolution est une nouvelle transformation.

> **BLOC doit pouvoir représenter un conflit avant de chercher à le résoudre.**

# 9. Ressources physiques

Séparation fondamentale :

```text
POLITIQUE
    ↓
Univers BLOC

MÉCANISME / CONTRAINTE
    ↓
BLOC Machine + hardware
```

Les décisions de priorité, équité, importance ou économie d'énergie doivent autant que possible être exprimées dans BLOC.

La machine impose uniquement les contraintes réellement physiques et l'isolation nécessaire.

# 10. SENSE et ACTUATE

```text
        MONDE PHYSIQUE
          ↑       │
          │       ↓
      ACTUATE    SENSE
          │       │
          └───┬───┘
              │
        UNIVERS BLOC
```

**SENSE** : une réalité physique produit une observation BLOC.

**ACTUATE** : une structure BLOC autorisée provoque un effet physique.

Cela généralise clavier, souris, caméra, réseau, stockage, GPU, écran, capteurs et actionneurs.

Un driver devient conceptuellement un adaptateur entre protocole physique et structures BLOC.

# 11. Sécurité

Ne pas introduire immédiatement `USER`, `ROOT`, `ACL` ou `ROLE`.

L'hypothèse est une sécurité fondée sur :

```text
IDENTITÉ
CAPACITÉ
PROVENANCE
CONTEXTE
```

Une action doit pouvoir fournir une preuve structurelle qu'elle est admissible.

Les capacités peuvent être accordées, déléguées, limitées ou révoquées.

Une racine de confiance physique ou cryptographique sera néanmoins nécessaire pour empêcher qu'un agent forge lui-même son autorité.

# 12. Processus émergent

Un processus classique peut être réinterprété comme :

> **un Bloc agent opérant dans un contexte limité par des capacités, produisant des transformations.**

Ainsi, `PROCESS` n'est pas requis comme primitive ontologique.

# 13. Mémoire

> **Il n'existe qu'une mémoire logique : l'univers BLOC. Les types de mémoire classiques sont des stratégies de matérialisation.**

Un Bloc logique peut être matérialisé en RAM, sur SSD, répliqué, distant, archivé ou absent physiquement mais reconstructible.

Ne pas confondre :

```text
existence logique
matérialisation physique
disponibilité actuelle
```

# 14. Reconstructibilité et cache

Si C est entièrement dérivable de A, B et d'une règle R :

```text
A + B + R → C
```

C n'a pas nécessairement besoin d'être conservé physiquement.

Un cache devient :

> **une matérialisation temporaire d'une structure qui peut être retrouvée ou reconstruite.**

Swap, cache et archive deviennent des politiques de matérialisation.

# 15. Identité et matérialisation

Une identité logique peut posséder plusieurs matérialisations :

```text
          B42
       /   |   \
      P1   P2   P3
     RAM  SSD  distant
```

La destruction de P1 ne détruit pas nécessairement B42.

Cette distinction permet d'unifier réplication, cache, sauvegarde, stockage distant et rematérialisation.

# 16. Oubli

Le principe append-only doit être formulé précisément :

> **Une transformation ne réécrit pas silencieusement l'histoire.**

Un oubli explicite peut exister.

Il peut conserver la trace qu'un oubli a eu lieu tout en rendant l'information antérieure non reconstructible.

# 17. Fichiers, dossiers et chemins

Un fichier n'est pas une primitive.

> **Un fichier est une interprétation stable d'un sous-graphe.**

Un dossier, une collection, un tag ou un workspace peuvent être différentes interprétations de relations contextuelles.

> **Un chemin décrit une manière d'atteindre un Bloc ; il n'est pas son identité.**

Un même objet peut être accessible par plusieurs parcours sans duplication logique.

# 18. Formats

Le format peut être une matérialisation ou une projection.

```text
Document logique
    ├── projection PDF
    ├── projection texte
    └── autre matérialisation
```

Les représentations ne sont pas automatiquement équivalentes : BLOC doit pouvoir exprimer dérivation, projection, approximation ou perte d'information.

Pour les objets binaires volumineux, l'implémentation peut utiliser des blobs compacts.

# 19. Applications et exécutables

Code et données ne sont pas ontologiquement séparés.

Un programme peut être un sous-graphe de règles interprétables par le moteur.

Une application peut émerger comme :

```text
lentilles
+
vues
+
règles
+
capacités
+
contexte
```

L'utilisateur n'est donc pas obligé de penser « ouvrir un fichier avec une application ».

# 20. Compatibilité

Les abstractions classiques peuvent être réintroduites comme lentilles de compatibilité :

```text
BLOC → POSIX lens → filesystem / interfaces attendues
BLOC → SQL lens → tables
BLOC → document lens → document
BLOC → object-storage lens → objets
```

Cela permet d'utiliser des systèmes existants sans laisser leurs abstractions dicter celles du Core.

# 21. Communication

> **Communiquer, dans BLOC, consiste à modifier la causalité et/ou la visibilité d'une information entre contextes.**

Localement, aucune copie n'est nécessaire si deux agents peuvent observer la même structure.

Un message peut être une structure produite par A et observable par B.

Un message traité n'est pas nécessairement supprimé : une nouvelle structure peut représenter son traitement.

# 22. Abstractions de communication

```text
shared memory
= plusieurs agents observent la même structure

message
= une structure devient observable par un destinataire

RPC
= demande + réponse causale

event bus
= plusieurs agents observent une même production

pipe / stream
= chaîne causale ordonnée de productions

socket
= transport physique de structures entre frontières
```

Une API devient une convention de motifs reconnus et produits.

Un protocole peut lui-même être décrit dans BLOC.

# 23. Distribution

```text
Univers A
   ↓
ACTUATE réseau
════════════════
SENSE réseau
   ↓
Univers B
```

Le transport physique ne doit pas changer l'identité logique si le système considère qu'il s'agit du même Bloc.

Les branches concurrentes créées hors connexion peuvent coexister puis être fusionnées explicitement.

# 24. Interface humain-machine

Distinguer :

```text
SIGNAL
  ↓
OBSERVATION
  ↓
INTERPRÉTATION
  ↓
INTENTION
  ↓
TRANSFORMATION
```

Un clic physique n'est pas une intention.

Une coordonnée n'acquiert un sens qu'avec une surface, une vue et un contexte.

Clavier, tactile, voix ou IA sont différentes voies permettant de produire des intentions interprétables.

# 25. Fenêtres, bureau, CLI et GUI

Une fenêtre peut émerger comme une vue bornée sur un contexte.

Un bureau peut émerger comme une organisation de vues.

CLI, GUI, voix et interface conversationnelle peuvent être différentes lentilles d'interaction sur les mêmes opérations logiques.

> **Le graphe est une lentille parmi d'autres.**

# 26. Boot

Le démarrage doit être pensé comme l'amorçage progressif d'un univers.

```text
POWER
 ↓
Hardware
 ↓
BLOC Machine
 ↓
Seed
 ↓
Univers minimal
 ↓
découverte des capacités
 ↓
expansion de l'univers
 ↓
règles / politiques
 ↓
contextes
 ↓
interfaces
 ↓
interaction
```

Le **Seed** est le premier sous-graphe dont la machine connaît la matérialisation au démarrage.

Il doit être aussi petit que possible et fournir assez de conventions pour permettre à BLOC de construire des règles plus riches à l'intérieur de BLOC.

# 27. Boot causal

Les capacités peuvent émerger selon leurs dépendances :

```text
          Seed
       /   |   \
 stockage GPU réseau
     ↓      ↓
 règles   affichage
       \   /
       contexte
```

Le système n'a pas nécessairement un instant universel « boot terminé ».

Il atteint progressivement des **états de capacité**.

# 28. Cycle complet

```text
POWER
 ↓
BLOC Machine
 ↓
Seed
 ↓
Univers minimal
 ↓
capacités physiques
 ↓
règles et politiques
 ↓
contextes
 ↓
lentilles et vues
 ↓
interaction
 ↓
transformations causales
 ↓
mémoire / matérialisation
 ↓
persistance
 ↓
arrêt
 ↓
nouvel amorçage
```

# 29. Constitution minimale de BLOC OS

### Axiome 1 — Identité
Le Bloc fondamental est une identité.

### Axiome 2 — Relation
La complexité émerge de relations entre identités.

### Axiome 3 — Interprétation
Type, contenu, sens et rôle ne sont pas intrinsèques ; ils émergent de l'interprétation.

### Axiome 4 — Transformation explicite
L'évolution produit de nouvelles structures causales au lieu de réécrire silencieusement l'histoire.

### Axiome 5 — Exécution structurelle
L'exécution minimale est recherchée sous la forme `MATCH → PRODUCE`.

### Axiome 6 — Temps causal
La causalité précède la chronologie globale.

### Axiome 7 — Contexte
Observation et action sont susceptibles d'être limitées par un contexte et des capacités.

### Axiome 8 — Séparation logique / physique
Une identité logique est distincte de ses matérialisations physiques.

### Axiome 9 — Frontière physique
Le monde extérieur rencontre BLOC par observation (`SENSE`) et matérialisation/action (`ACTUATE`).

### Axiome 10 — Minimalité permanente
Aucune abstraction ne doit entrer dans le cœur si elle peut émerger de la structure existante.

# 30. Ce que le Core ne doit pas connaître a priori

À ce stade, aucune nécessité conceptuelle n'a imposé comme primitive :

```text
FILE
DIRECTORY
PATH
PROCESS
THREAD
USER
ROOT
ACL
APP
WINDOW
DESKTOP
MESSAGE
SOCKET
DATABASE
TABLE
CACHE
SWAP
CLOUD
AI
```

Ces concepts peuvent être utiles ; ils doivent simplement être construits **au-dessus** tant que l'expérience ne démontre pas qu'ils sont fondamentaux.

# 31. Questions critiques à tester

1. Formaliser exactement le modèle minimal d'identité et de lien.
2. Définir une convention bootstrap minimale pour `MATCH → PRODUCE`.
3. Vérifier l'expressivité sur des exemples calculatoires non triviaux.
4. Définir déterminisme, matches concurrents et garanties de convergence.
5. Tester la sécurité par capacités et provenance.
6. Définir l'identité distribuée et la réplication.
7. Définir compaction, oubli et reconstruction.
8. Mesurer le coût réel d'un prototype.
9. Construire un Seed minimal.
10. Faire émerger une première interface sans coder « fichier », « processus » ou « application » dans le Core.

# 32. Règle de développement

Chaque ajout au Core doit être accompagné de cette justification :

> **Pourquoi ce mécanisme ne peut-il pas être représenté ou émerger avec les primitives déjà disponibles ?**

L'absence de réponse convaincante signifie que le mécanisme doit rester au-dessus du Core.

# 33. Direction d'expérimentation

```text
formalisation
↓
simulateur BLOC Machine
↓
Seed minimal
↓
moteur MATCH → PRODUCE
↓
graphe causal persistant
↓
lentilles
↓
agents / capacités
↓
interface expérimentale
↓
adaptateurs système existant
↓
prototype bootable
```

Une implémentation Linux ou Docker peut servir de laboratoire transportable.

Le modèle théorique doit toutefois rester indépendant de Linux afin qu'un futur runtime natif ou bootable reste possible.

# 34. Formule directrice

> **BLOC OS n'est pas un système où le noyau gère une liste d'objets informatiques prédéfinis.**
>
> **C'est une machine minimale maintenant un univers d'identités et de relations, capable d'évoluer par transformations causales, tandis que fichiers, processus, applications, utilisateurs, interfaces et agents émergent comme interprétations de cet univers.**

Et la règle qui doit continuer à guider tout le projet reste :

> **Tout est Bloc. Le reste n'est qu'interprétation.**
