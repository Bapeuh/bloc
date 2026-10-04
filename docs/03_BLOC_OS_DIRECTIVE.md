# Directive d'architecture — BLOC OS

## 0. Objet

Ce document synthétise l'exploration conceptuelle de BLOC appliquée à un système d'exploitation.

Il ne constitue pas une spécification finale. Il définit une direction, des invariants souhaités et des hypothèses à éprouver.

Une correction majeure est désormais assumée :

> **Le cœur intellectuel de BLOC est une ontologie minimale ; le cœur opérationnel de BLOC OS est la BLOC Machine qui doit réellement la garantir.**

La machine doit donc être spécifiée et évaluée avec autant de rigueur que l'ontologie.

---

# 1. Couches à distinguer

~~~text
UNIVERS BLOC
Blocs, relations, causalité, structures interprétables
        │
        ▼
RULE ENGINE
langage de motifs + production de transformations
        │
        ▼
BLOC MACHINE
runtime de confiance : atomicité, protection, médiation,
identités, stockage logique, interaction avec le substrat
        │
        ▼
SUBSTRAT D'EXÉCUTION
CPU, MMU, interruptions, timers, mémoire physique, pilotes,
ordonnancement bas niveau, primitives d'I/O
~~~

Le Seed est un graphe initial chargé par la Machine. Il n'est ni une couche matérielle ni une primitive fondamentale.

Le Trust Anchor est séparé du Seed : il fonde certaines garanties non forgeables.

# 2. Ontologie minimale

Hypothèse de travail :

> **Un Bloc fondamental est une identité sans contenu intrinsèque.**

Valeur, type, rôle, propriété et sens doivent émerger de structures relationnelles et de leur interprétation.

Cela définit le modèle logique. Cela ne dit pas encore comment la machine le garantit efficacement.

# 3. MATCH → PRODUCE devient une abstraction à spécifier

La formule reste utile :

~~~text
MATCH → PRODUCE
~~~

mais elle ne doit plus être comprise comme « rechercher n'importe quel sous-graphe arbitraire dans tout l'univers ».

Le Rule Engine devra utiliser un langage de motifs explicitement restreint.

Candidats à imposer :

- motifs ancrés ;
- voisinage local ;
- profondeur bornée ;
- relations indexables ;
- variables limitées ;
- prédicats positifs par défaut ;
- coût estimable ;
- budget d'exécution ;
- règles de terminaison définies.

Le langage exact reste à spécifier dans 04_BLOC_MACHINE_SPEC.md.

# 4. Redéclenchement et idempotence

Dans un univers append-only, un motif positif peut rester vrai indéfiniment.

Une règle naïve :

~~~text
MATCH A
→ PRODUCE B
~~~

peut donc se déclencher sans fin.

Une piste à formaliser consiste à donner à chaque application logique une identité déterministe :

~~~text
ApplicationID = F(Règle, Match canonique)
~~~

Deux tentatives portant sur la même application logique convergeraient vers la même identité.

Cela peut fournir une idempotence logique, mais ne supprime pas les besoins physiques d'atomicité, de déduplication ou de coordination.

Aucune solution n'est encore considérée comme acquise.

# 5. Monotonicité et coordination

BLOC ne doit plus être présenté comme pouvant éviter toute coordination.

Les calculs purement monotones peuvent souvent progresser sans coordination globale.

En revanche, des opérations telles que :

- débit sur un solde limité ;
- allocation unique ;
- réservation exclusive ;
- révocation ;
- consommation unique ;
- test d'absence ;
- invariant global ;

peuvent exiger ordre, sérialisation, consensus ou autorité unique.

Principe retenu :

> **BLOC cherche à rendre la coordination explicite et locale aux invariants qui l'exigent, pas à la supprimer universellement.**

# 6. Conflits

Le graphe peut représenter plusieurs branches concurrentes :

~~~text
    A
   / \
  B   C
~~~

Cela reste une propriété importante.

Mais :

> **représenter un conflit n'est pas résoudre un invariant.**

Pour des données purement logiques, plusieurs branches peuvent coexister.

Pour un effet externe ou une ressource exclusive, la Machine ou une autorité supérieure doit parfois empêcher certaines branches avant qu'elles ne deviennent des effets réels.

# 7. Temps, concurrence et ordre physique

La causalité reste plus fondamentale que l'ordre chronologique global dans le modèle logique.

Cependant la Machine doit posséder un ordre physique suffisant pour :

- garantir certaines sections atomiques ;
- arbitrer des accès concurrents ;
- protéger des invariants ;
- interagir avec le matériel.

Le modèle ne doit donc pas confondre ordre logique causal et ordre physique nécessaire à l'exécution.

# 8. SENSE et ACTUATE

SENSE et ACTUATE sont des interfaces de frontière, pas des axiomes ontologiques.

## SENSE

Transforme ou associe une observation du monde externe à une structure BLOC.

Une observation externe peut être non reproductible.

## ACTUATE

Ne doit plus être défini comme « une structure provoque un effet ».

La chaîne correcte est au minimum :

~~~text
Intention
   ↓
Médiation / autorisation
   ↓
Tentative d'effet
   ↓
Monde physique
   ↓
Observation de résultat
~~~

Résultats possibles :

~~~text
confirmé
échoué
inconnu
~~~

Le cas inconnu est essentiel en présence de pannes ou de réseau.

# 9. Exactly-once n'est pas une garantie fondamentale

BLOC ne promet pas un effet physique exactement une fois par magie.

Selon le domaine, il pourra être nécessaire d'utiliser :

- identifiant d'opération ;
- opération idempotente ;
- déduplication ;
- journal d'intention ;
- accusé de réception ;
- protocole transactionnel ;
- compensation ;
- autorité de sérialisation.

La stratégie appartient au protocole ou à une couche de service, sauf si l'expérience démontre qu'une garantie plus forte doit entrer dans la Machine.

# 10. Fast paths hors graphe

Tout ce qui existe dans le système n'a pas besoin d'être exécuté via le Rule Engine.

Des chemins bas niveau peuvent rester hors du graphe :

- interruptions ;
- MMU / défauts mémoire ;
- scheduling CPU bas niveau ;
- watchdogs ;
- certaines opérations temps réel ;
- primitives d'isolation ;
- gestion immédiate de certains périphériques.

Ces événements peuvent ensuite être observés ou projetés dans BLOC si nécessaire.

Principe :

> **Tout peut être représenté en BLOC sans que toute opération physique doive être exécutée par transformation du graphe.**

# 11. Sécurité et Médiateur d'accès

La confidentialité doit être appliquée avant observation.

Un composant de confiance est donc explicitement reconnu :

> **Médiateur d'accès**

Il intervient avant qu'une structure protégée soit rendue visible ou qu'une action privilégiée soit effectuée.

Il peut évaluer :

- identité ;
- capacité d'autorité ;
- contexte ;
- provenance ;
- statut de révocation ;
- politique active.

Les politiques peuvent être représentées dans BLOC.

L'enforcement appartient à la Trusted Computing Base.

# 12. Révocation

L'append-only préserve l'historique des grants et révocations, mais ne résout pas automatiquement leur validité courante.

Une autorisation doit être réévaluée lors de son usage.

Le modèle de révocation reste à choisir parmi des approches comme :

- epochs ;
- version d'autorité ;
- durée de validité ;
- indirection ;
- autorité de validation ;
- autre structure dédiée.

# 13. Mémoire et reconstructibilité

Principe conservé :

> **Il existe une mémoire logique ; RAM, SSD, cache, archive et réseau sont des stratégies de matérialisation.**

Mais une structure n'est reconstructible que si :

1. toutes ses dépendances nécessaires sont encore accessibles ;
2. sa dérivation est pure ou suffisamment déterministe ;
3. aucune observation externe indispensable n'a été perdue.

Il faut distinguer dérivation pure et transformation dépendant de SENSE, du réseau, du temps ou du hasard.

# 14. Oubli et contamination informationnelle

Supprimer la source d'une information ne suffit pas si ses dérivés permettent de la reconstruire.

L'oubli peut donc nécessiter :

~~~text
source
↓
analyse de provenance
↓
dérivés dépendants
↓
effacement / invalidation / reclassification
~~~

La provenance devient simultanément :

- outil d'audit ;
- outil de reconstruction ;
- outil de suivi de contamination ;
- possible source de fuite.

Elle doit elle-même être soumise au contrôle d'accès.

# 15. Ressources et politique

La séparation reste :

~~~text
POLITIQUE
    ↓
Univers BLOC

ENFORCEMENT / MÉCANISME
    ↓
BLOC Machine + substrat
~~~

Mais certaines garanties de sécurité, d'atomicité et d'isolation appartiennent nécessairement à la Machine.

# 16. Fichiers, processus, applications

Les conclusions précédentes restent valables comme hypothèses d'émergence :

- fichier = interprétation stable d'un sous-graphe ;
- chemin = manière d'atteindre une identité ;
- processus = agent + contexte + droits + exécution ;
- application = lentilles + vues + règles + contexte ;
- message = structure rendue observable dans un autre contexte ;
- socket = mécanisme de transport physique exposant une abstraction de communication.

Ces abstractions restent au-dessus de la Machine tant qu'aucune nécessité expérimentale ne force leur descente.

# 17. Boot

Le boot reste un amorçage :

~~~text
POWER
↓
substrat
↓
BLOC Machine
↓
Trust Anchor
↓
Seed
↓
Rule Engine / règles initiales
↓
univers élargi
↓
services et contextes
~~~

Mais certains mécanismes nécessaires au boot et à la sûreté peuvent exister hors graphe avant que le Seed soit utilisable.

# 18. Vocabulaire normalisé

| Terme | Sens |
|---|---|
| Bloc | identité logique fondamentale |
| Core Model | sémantique de l'univers BLOC |
| Rule Engine | moteur du langage de règles |
| BLOC Machine | runtime de confiance |
| Seed | graphe initial |
| Trust Anchor | racine de confiance |
| Médiateur d'accès | enforcement avant observation/action |
| Capacité d'autorité | droit/preuve non forgeable |
| Condition de disponibilité | condition d'activation |
| Ressource physique | ressource matérielle finie |
| Matérialisation | représentation physique d'un objet logique |
| Dérivation pure | calcul reproductible depuis dépendances explicites |
| Observation externe | information issue du monde hors graphe |

Le mot « capacité » seul doit être évité lorsqu'il peut être ambigu.

# 19. Constitution minimale révisée

Les axiomes de BLOC OS doivent rester ontologiques :

1. **Identité** — le Bloc fondamental est une identité.
2. **Minimalité sémantique** — le Bloc n'a pas intrinsèquement type, contenu ou rôle.
3. **Relation** — les structures émergent de relations.
4. **Interprétation** — sens et type émergent du contexte.
5. **Transformation explicite** — les évolutions ne réécrivent pas silencieusement l'histoire.
6. **Causalité** — la dépendance causale prime sur un ordre total.
7. **Multiplicité des interprétations** — une structure peut avoir plusieurs lectures.
8. **Séparation logique / physique** — identité et matérialisation sont distinctes.

Les éléments suivants ne sont plus appelés axiomes :

- SENSE / ACTUATE : interfaces de frontière ;
- minimalité permanente : règle de conception ;
- matching : mécanisme du Rule Engine ;
- contrôle d'accès : garantie de la Machine ;
- Trust Anchor : mécanisme de sécurité.

# 20. Ce que nous savons désormais ne pas pouvoir éluder

La BLOC Machine devra probablement avoir une réponse explicite pour :

- allocation et unicité des identités ;
- stockage et accès aux liens ;
- atomicité minimale ;
- concurrence ;
- restrictions du matching ;
- idempotence des applications ;
- gestion de l'absence / non-monotonicité ;
- coordination lorsque nécessaire ;
- médiation d'accès ;
- révocation ;
- isolation ;
- fast paths hardware ;
- SENSE ;
- ACTUATE ;
- résultat d'effet inconnu ;
- horloge et timers physiques ;
- Trust Anchor ;
- persistance minimale ;
- récupération après panne.

Ce coût fait partie de BLOC.

# 21. Questions critiques prioritaires

La priorité n'est plus d'étendre l'OS vers de nouvelles abstractions.

Elle est de spécifier la Machine :

1. Quel est son état minimal ?
2. Quelles opérations sont atomiques ?
3. Comment une identité est-elle créée ?
4. Qu'est-ce qu'un lien physiquement ?
5. Quel langage exact de MATCH est autorisé ?
6. Comment une application de règle est-elle identifiée ?
7. Comment évite-t-on le redéclenchement infini ?
8. Quelle négation est admise ?
9. Quand la coordination devient-elle obligatoire ?
10. Que garantit le Médiateur d'accès ?
11. Comment fonctionne la révocation ?
12. Quels chemins matériels restent hors graphe ?
13. Quelle sémantique exacte possède ACTUATE ?
14. Comment récupérer après crash ?
15. Que garantit le Seed et que doit garantir la Machine avant lui ?

Le document de travail correspondant est 04_BLOC_MACHINE_SPEC.md.

# 22. Formule directrice révisée

> **BLOC propose une ontologie minimale d'identités et de relations, mais reconnaît qu'une machine réelle doit payer explicitement le coût de l'exécution, de la sécurité, de l'atomicité, de la coordination et du monde physique.**

Et la phrase fondatrice reste :

> **Tout est Bloc. Le reste n'est qu'interprétation.**

Avec une précision désormais essentielle :

> **Tout peut être représenté comme Bloc ; tout n'a pas besoin d'être exécuté comme transformation de graphe.**
