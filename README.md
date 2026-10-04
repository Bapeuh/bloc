# BLOC

> **Tout est Bloc. Le reste n'est qu'interprétation.**

BLOC est une recherche d'architecture numérique fondée sur un substrat volontairement minimal : des identités, des relations et des mécanismes d'interprétation capables de faire émerger des structures plus complexes.

Le projet ne part pas des abstractions classiques — fichier, processus, application, utilisateur, table, document — comme primitives. Il cherche au contraire à déterminer lesquelles peuvent émerger d'un modèle plus simple.

## Documents fondateurs

- [Manifeste](docs/01_MANIFESTE.md) — identité et philosophie du projet.
- [Constitution et définitions](docs/02_CONSTITUTION_ET_DEFINITIONS.md) — vocabulaire, axiomes et règles de conception.
- [Directive BLOC OS](docs/03_BLOC_OS_DIRECTIVE.md) — exploration du système d'exploitation construit selon la logique BLOC.
- [BLOC Machine Spec](docs/04_BLOC_MACHINE_SPEC.md) — document de travail consacré à la machine réelle qui doit garantir le modèle.

## Principe de conception

Lorsqu'un nouveau concept apparaît, poser systématiquement la question :

> **Est-il réellement fondamental, ou peut-il être représenté comme une structure de Blocs et de relations interprétée par une lentille ?**

S'il peut émerger, il ne doit pas devenir une primitive de l'ontologie.

Mais cette règle ne doit pas masquer le coût réel de l'exécution :

> **Ontologie minimale ne signifie pas Machine minimale.**

Tout mécanisme nécessaire pour garantir correctement le modèle — atomicité, contrôle d'accès, ordonnancement physique, isolation, accès au matériel, gestion des interruptions, matching, etc. — doit être explicitement compté dans la BLOC Machine ou dans son substrat d'exécution.

## Statut

BLOC est actuellement au stade de formalisation conceptuelle et de spécification de sa machine d'exécution.

La priorité n'est plus d'ajouter de nouvelles abstractions supérieures, mais de préciser :

- ce que la BLOC Machine garantit ;
- ce qu'elle ne garantit pas ;
- le langage exact de règles ;
- les limites de MATCH → PRODUCE ;
- le modèle de concurrence et de coordination ;
- le contrôle d'accès avant observation ;
- la frontière entre graphe logique et fast paths matériels ;
- la sémantique de SENSE et ACTUATE.

La prochaine étape de travail est donc [04_BLOC_MACHINE_SPEC.md](docs/04_BLOC_MACHINE_SPEC.md).
