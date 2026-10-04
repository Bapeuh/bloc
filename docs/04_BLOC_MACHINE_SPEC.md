# BLOC Machine Specification — document de travail

> **Statut : brouillon à remplir.**
>
> Ce document doit devenir la spécification opérationnelle de la BLOC Machine.
> Il ne doit pas chercher à préserver artificiellement la simplicité du modèle.
> Toute garantie réelle nécessaire au fonctionnement de BLOC doit être explicitement nommée ici.

---

# 0. Objectif

Décrire la plus petite machine réelle capable de garantir correctement le Core Model BLOC.

Question centrale :

> **Quelle est la plus petite Trusted Computing Base capable de rendre l'univers BLOC cohérent, sûr et exécutable ?**

# 1. Périmètre

## 1.1 Ce que la BLOC Machine garantit

À remplir.

Candidats à évaluer :

- unicité ou domaine d'unicité des identités ;
- intégrité des liens ;
- atomicité minimale ;
- médiation d'accès ;
- isolation ;
- persistance minimale ;
- récupération après panne ;
- interaction avec le Rule Engine ;
- interaction avec le hardware.

## 1.2 Ce qu'elle ne garantit pas

À remplir.

Exemples à trancher :

- absence de coordination globale ;
- exactly-once externe ;
- terminaison de toute règle ;
- déterminisme global ;
- disponibilité réseau ;
- conservation éternelle ;
- convergence automatique de tous conflits.

# 2. État minimal de la Machine

Définir exactement l'état que la Machine doit conserver.

Questions :

- table d'identités ?
- index de liens ?
- journal ?
- epochs ?
- espace d'autorité ?
- état du Rule Engine ?
- état de récupération ?
- clés du Trust Anchor ?

Ne rien ajouter sans justification.

# 3. Identité

## 3.1 Création

À définir.

## 3.2 Domaine d'unicité

À définir :

- local ;
- global ;
- cryptographique ;
- namespaced ;
- autre.

## 3.3 Collision

À définir.

## 3.4 Réplication de la même identité

À définir.

# 4. Lien

Définir le lien physique minimal.

Questions :

- non orienté ou orienté physiquement ?
- tuple fixe ?
- relation par identité intermédiaire ?
- mutabilité physique autorisée ?
- intégrité référentielle ?
- index obligatoires ?

# 5. Opérations primitives de la Machine

Lister uniquement les opérations réellement nécessaires.

| Primitive | Entrées | Sortie | Atomicité | Peut échouer ? | Autorisation requise ? |
|---|---|---|---|---|---|
| create_identity | ? | ? | ? | ? | ? |
| create_link | ? | ? | ? | ? | ? |
| observe | ? | ? | ? | ? | ? |
| ... | | | | | |

Question importante :

> MATCH appartient-il à la Machine ou exclusivement au Rule Engine ?

# 6. Atomicité

Définir le plus petit domaine atomique nécessaire.

Questions :

- création identité seule ?
- identité + liens ?
- production complète d'une règle ?
- transaction multi-structures ?
- atomicité locale seulement ?
- journal write-ahead ?

# 7. Rule Engine

## 7.1 Séparation avec la Machine

À définir.

## 7.2 Langage de MATCH

Le matching arbitraire de sous-graphe est exclu par défaut.

Restrictions candidates :

- motif ancré ;
- profondeur maximale ;
- nombre maximal de variables ;
- index requis ;
- absence de récursion implicite ;
- budget d'exécution ;
- prédicats positifs seulement par défaut.

Décision à prendre pour chaque point.

## 7.3 PRODUCE

Définir précisément ce qu'une règle peut produire.

## 7.4 Négation

Questions :

- NOT EXISTS existe-t-il ?
- sous quelles restrictions ?
- nécessite-t-il une barrière de coordination ?
- comment est définie la complétude de l'espace observé ?

# 8. Application d'une règle

## 8.1 Identité d'application

Piste :

~~~text
ApplicationID = F(rule_id, canonical_match)
~~~

À valider ou rejeter.

## 8.2 Idempotence

Définir.

## 8.3 Redéclenchement

Définir comment éviter :

~~~text
A → B1
A → B2
A → B3
...
~~~

lorsque le même match reste vrai.

## 8.4 Canonicalisation du match

À définir.

# 9. Terminaison

La Machine ou le Rule Engine garantit-il :

- terminaison par règle ?
- quota ?
- budget CPU ?
- profondeur ?
- nombre de productions ?
- détection de cycles ?

À remplir.

# 10. Déterminisme

Définir :

- déterminisme d'une règle ;
- déterminisme d'un ensemble de règles ;
- ordre des matches ;
- ordre des productions ;
- reproductibilité ;
- rôle du hasard et du temps.

# 11. Concurrence

## 11.1 Matches concurrents

À définir.

## 11.2 Productions concurrentes

À définir.

## 11.3 Conflits d'écriture physique

À définir.

## 11.4 Concurrence logique vs ordre physique

Formaliser la distinction.

# 12. Monotonicité

Classifier les opérations :

- monotones ;
- non monotones ;
- nécessitant test d'absence ;
- nécessitant coordination.

Objectif :

> rendre visible dans le langage quelles règles peuvent être évaluées sans coordination globale.

# 13. Coordination

Définir les mécanismes disponibles.

Candidats :

- sérialisation locale ;
- lock ;
- compare-and-swap ;
- lease ;
- leader ;
- quorum ;
- consensus ;
- autorité unique ;
- protocole applicatif.

Ne pas imposer un mécanisme plus fort que nécessaire.

# 14. Médiateur d'accès

## 14.1 Position

Toute observation protégée doit passer par lui avant divulgation.

## 14.2 Entrées

À définir :

- identité demandeuse ;
- capacité d'autorité ;
- contexte ;
- politique ;
- provenance ;
- epoch / statut actuel.

## 14.3 Décision

À définir.

## 14.4 Cache de décision

À définir avec prudence vis-à-vis des révocations.

# 15. Capacité d'autorité

Définir :

- création ;
- délégation ;
- portée ;
- durée ;
- atténuation ;
- preuve ;
- non-forgeabilité.

# 16. Révocation

Questions obligatoires :

- vérification à chaque usage ?
- epoch ?
- version ?
- expiration ?
- liste de révocation ?
- autorité en ligne ?
- propagation distribuée ?
- comportement hors connexion ?

# 17. Trust Anchor

Définir ce qui doit être non forgeable.

Questions :

- identité machine ?
- clés racines ?
- boot mesuré ?
- secrets du Médiateur ?
- horloge protégée ?
- compteur monotone ?

# 18. SENSE

Définir l'interface exacte.

Pour chaque observation :

- source ;
- instant physique éventuel ;
- identité logique créée ou associée ;
- preuve d'origine ;
- reproductibilité ;
- niveau de confiance.

Distinguer impérativement observation externe et dérivation pure.

# 19. ACTUATE

Définir le protocole minimal :

~~~text
Intent
↓
Authorize
↓
Attempt
↓
External world
↓
Observe result
~~~

États possibles :

- pending ;
- confirmed ;
- failed ;
- unknown ;
- compensated.

Décider lesquels appartiennent au Core Model et lesquels sont seulement conventions.

# 20. Idempotence externe

Définir les mécanismes disponibles :

- operation_id ;
- déduplication ;
- nonce ;
- protocole idempotent ;
- journal ;
- compensation.

Ne pas promettre exactly-once sans preuve.

# 21. Fast paths matériels

Lister les mécanismes qui restent hors Rule Engine.

Candidats :

- interruption ;
- exception CPU ;
- MMU ;
- page fault ;
- scheduling physique ;
- watchdog ;
- DMA ;
- timers bas niveau ;
- certaines boucles temps réel.

Pour chacun :

1. pourquoi il reste hors graphe ;
2. ce qui est tout de même observé dans BLOC ;
3. quelles garanties il doit respecter.

# 22. Horloge et temps physique

Définir :

- horloge monotone ;
- horloge murale ;
- confiance ;
- dérive ;
- ordre physique ;
- relation avec causalité logique.

# 23. Mémoire et matérialisation

Définir :

- représentation RAM ;
- stockage durable ;
- index ;
- réplication ;
- migration ;
- cache ;
- reconstruction.

La matérialisation ne doit pas devenir une propriété intrinsèque du Bloc.

# 24. Reconstructibilité

Définir les critères exacts.

Une structure est reconstructible seulement si :

- ses dépendances sont connues ;
- elles existent encore ;
- la règle est reproductible ;
- aucune observation externe manquante n'est nécessaire.

# 25. Oubli

Définir :

- oubli logique ;
- oubli physique ;
- oubli cryptographique éventuel ;
- dérivés ;
- provenance ;
- caches ;
- réplications ;
- backups.

Question centrale :

> Comment prouver qu'une information n'est plus reconstructible ?

# 26. Provenance et information flow

Définir :

- granularité de provenance ;
- confidentialité de la provenance ;
- propagation de labels de sensibilité ;
- dérivés contaminés ;
- déclassification éventuelle.

# 27. Persistance et crash recovery

Définir :

- quelles structures doivent survivre ;
- point de commit ;
- journalisation ;
- replay ;
- productions partiellement écrites ;
- effet ACTUATE confirmé mais graphe non mis à jour ;
- graphe mis à jour mais effet externe inconnu.

# 28. Seed

Définir précisément :

- format ;
- localisation ;
- authentification ;
- contenu minimal ;
- version ;
- compatibilité ;
- mécanisme de chargement.

Le Seed ne doit pas contenir implicitement des garanties que la Machine devrait fournir.

# 29. Boot

Formaliser :

~~~text
POWER
↓
substrat minimal
↓
Trust Anchor
↓
BLOC Machine
↓
validation Seed
↓
Rule Engine
↓
univers actif
~~~

Préciser quelles étapes sont hors BLOC.

# 30. Invariants de la Machine

À écrire sous forme testable.

Exemple de format :

~~~text
INV-001:
Aucune observation protégée n'est remise à un agent
avant validation par le Médiateur d'accès.
~~~

# 31. Propriétés volontairement non garanties

Créer une liste explicite.

Cette section est essentielle pour éviter les promesses implicites.

# 32. Scénarios adversariaux à tester

Au minimum :

1. deux CPUs appliquent la même règle ;
2. une règle matche éternellement ;
3. deux agents débitent la même ressource ;
4. révocation pendant l'usage ;
5. crash pendant PRODUCE ;
6. crash après ACTUATE mais avant confirmation ;
7. duplication réseau ;
8. partition réseau ;
9. oubli d'une donnée avec dérivés ;
10. fuite via provenance ;
11. Seed corrompu ;
12. règle malveillante consommant toutes les ressources.

# 33. Critères pour BLOC Machine v0

La v0 ne sera considérée comme spécifiée que lorsque nous aurons au minimum :

- un état minimal formel ;
- un jeu de primitives ;
- un langage MATCH borné ;
- une sémantique PRODUCE ;
- un modèle d'atomicité ;
- un modèle de concurrence ;
- une stratégie d'idempotence ;
- une stratégie de coordination ;
- un Médiateur d'accès ;
- un modèle de révocation ;
- une sémantique SENSE/ACTUATE ;
- une liste de fast paths ;
- un modèle de crash recovery ;
- une liste d'invariants testables.

# 34. Règle de travail

Pour chaque mécanisme ajouté :

1. Quel problème concret résout-il ?
2. Pourquoi le Core Model seul ne suffit-il pas ?
3. Doit-il être dans la Machine, le Rule Engine, le Seed ou une couche supérieure ?
4. Quelle garantie ajoute-t-il ?
5. Quel coût et quelle complexité introduit-il ?
6. Peut-on le retirer sans casser un invariant ?

Le but n'est plus de produire la Machine la plus élégante sur le papier.

Le but est de produire :

> **la plus petite Machine dont les garanties suffisent réellement à faire exister BLOC.**
