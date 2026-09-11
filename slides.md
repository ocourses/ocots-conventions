<!-- LTeX: language=fr-FR -->

# Règles de rédaction — transparents

Support `beamer` + `\usepackage{ocots}` (le support est déduit de la classe).
Prérequis : [`communes.md`](communes.md).

> **État.** `SL1`–`SL8` sont toutes relues avec l'auteur et éprouvées sur
> `mesure-integration`.

Identifiants : `SL1`, `SL2`, … (voir [`README.md`](README.md#citer-une-règle--épingler-la-version)).

---

## SL1 — Le polycopié est la référence, dans un seul sens

**Tout ce qui est dans les transparents doit se retrouver dans le polycopié.
L'inverse n'importe pas** — le poly peut être plus riche, les transparents sont
une sélection.

C'est la règle qui structure les passes d'harmonisation : on inventorie ce qui
est en slide et absent du poly, et on le remonte dans le poly (jamais l'inverse).
Les notations et le vocabulaire suivent le poly (règle
[C2](communes.md#c2--cohérence-terminologique-et-notationnelle)).

### Ce que les transparents ont le droit de retirer

| Retiré | Gardé |
|---|---|
| les **preuves**, si le cours choisit de les renvoyer au poly (règle [SL5](#sl5--preuves-en-transparent--un-choix-de-cours-une-seule-bonne-façon)) | tout **énoncé** |
| les **corrigés** d'exercices | les **définitions** |
| les développements secondaires | la **progression** du cours |

## SL2 — Viser une numérotation identique au polycopié

**Objectif** : la Définition 2.3 des transparents est la Définition 2.3 du
polycopié. Même découpage en sections, mêmes numéros de définitions, de
théorèmes, de propositions.

C'est ce qui permet à un étudiant de passer de l'un à l'autre pendant le cours,
et à l'enseignant de dire « c'est la définition 2.3 » sans préciser laquelle des
deux numérotations.

**C'est un objectif, pas une contrainte absolue** — il n'est pas toujours
tenable, et les retraits autorisés par [SL1](#sl1--le-polycopié-est-la-référence-dans-un-seul-sens)
ne le mettent pas en péril, puisqu'ils ne portent pas sur des objets numérotés
(une preuve retirée ne décale rien ; un corrigé d'exercice non plus). Ce qui le
met en péril, c'est **une définition présente d'un seul côté**, ou **un
découpage de sections différent**.

Quand l'écart apparaît, c'est le signe qu'une notion manque au poly — et
[SL1](#sl1--le-polycopié-est-la-référence-dans-un-seul-sens) dit dans quel sens
le combler.

## SL3 — Une idée par diapositive

Une diapositive porte **un** énoncé, **un** exemple, **une** étape de
raisonnement. Deux idées → deux diapositives. Un `slide` qui déborde est un
`slide` à couper, pas à réduire en corps 7.

## SL4 — Le transparent n'est pas le polycopié

Le texte d'un transparent est **télégraphique** ; la narration est **orale**.
Conséquence : les règles d'amorce et de reprise du polycopié
([P3](poly.md#p3--une-phrase-damorce-motivée-avant-chaque-boîte) et
[P4](poly.md#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé)) **ne s'appliquent
pas** ici — c'est l'enseignant qui fait la liaison.

En revanche, la règle
[P2](poly.md#p2--pas-de-blocs-isolés-ou-enchaînés-sans-texte) survit sous une
autre forme : **une suite de boîtes sans titre ni fil visible** reste illisible
en projection. Le fil passe par les titres de diapositives — l'environnement
`slide{titre}` (pas `frame` nu) est ce qui les porte.

### Le corpus

Mesuré sur `mesure-integration` : **176 `slide` titrées contre 62 `frame`
nues** (74 %). Les `frame` nues ne sont pas des titres oubliés — toutes
partagent le même profil (`\vfill`, `\large`, `\vspace` en ouverture) : ce
sont les pages de titre et les diapositives de transition (« Le but de ce
chapitre est de… »), une catégorie à part qui n'a pas vocation à porter de
titre au sens de cette règle.

> **En attendant la révision du template.** Porter cette distinction par
> deux mécanismes (`slide` / `frame` nu) plutôt que par un seul avec titre
> optionnel est demandé au
> [chantier 6](template.md#chantier-6--un-seul-environnement-de-diapositive-titre-optionnel).

## SL5 — Preuves en transparent : un choix de cours, une seule bonne façon

**Inclure les preuves en slides ou non dépend du cours.** Certains renvoient
au polycopié et gardent l'idée ; d'autres — c'est le choix de
`mesure-integration` — veulent les preuves présentes, complètes. Cette règle
ne tranche pas ce choix, elle porte sur **ce qu'on fait une fois le choix
pris**.

**Une preuve qui tient sur une diapositive** : elle y va. **Une preuve qui
déborde** : elle se **fractionne** (`proofbegin` / `proofmiddle` / `proofend`),
jamais compressée en corps 7 pour tenir sur une seule — c'est le cas
particulier de [SL3](#sl3--une-idée-par-diapositive) appliqué à la preuve.

### Le corpus

`mesure-integration` fractionne largement : **40 preuves sur ~111** (36 %)
utilisent `proofbegin`/`proofmiddle`/`proofend`. Cohérent avec le choix du
cours — preuves gardées en entier, réparties sur autant de diapositives que
nécessaire plutôt que raccourcies.

## SL6 — `\pause` : progression, pas décoration

On découvre progressivement ce qui doit être **commenté** au fur et à mesure, ou
ce dont la surprise sert (un contre-exemple, un résultat inattendu). On ne met
pas de `\pause` sur une liste que l'on lit intégralement.

La remise à zéro des compteurs entre deux `\pause` est gérée par le support du
template — ne pas la bricoler dans le document.

> **Sans prise dans le corpus.** Zéro `\pause` sur `mesure-integration`, les 8
> chapitres. La règle n'a rien à quoi s'appliquer ici — sans prise, comme
> [TD7](td.md#td7--où-va-le-corrigé)/[TD8](td.md#td8--ne-pas-rouvrir-question-dans-un-solution)
> avant leur cas.

## SL7 — Figures lisibles en projection

Une figure de transparent n'est pas une figure de polycopié rétrécie :
épaisseurs de trait, taille des étiquettes et contraste se règlent **pour la
projection**. Vérifier en plein écran, pas dans l'aperçu.

Les figures des transparents vivent dans le dossier du chapitre
(`slides/chapitreN/figures/`) — une figure partagée avec le poly se **copie**,
elle ne se référence pas par un chemin relatif qui traverse les dossiers.

### Le corpus

Clean sur `mesure-integration` : tous les chemins de `\includegraphics` sont
des noms de fichiers simples, aucune traversée de dossier. `figures/` existe
dans les chapitres qui ont des images (1, 2, 3, 4, 8) ; 5, 6, 7 n'en ont
aucune — cohérent, pas une absence à corriger.

## SL8 — Le thème est un réglage global

`theme=ocots` par défaut ; `legacy-dark` reste disponible pour la projection.
Le choix se fait **au préambule**, une fois. Pas de couleur posée à la main dans
le corps d'une diapositive (`\slidecolor` existe pour changer la couleur
d'en-tête d'une série).

**Même principe pour la page de titre** : `\slidetitlepage` fournit le
mécanisme (logo via `\ocotslogos`, mis à jour si l'établissement change), on
ne le recompose pas à la main.

### Le corpus

Couleurs : clean — zéro `\textcolor`/`\color` posé à la main dans le corps
des diapositives, `theme=ocots` partout.

Page de titre : **6 chapitres sur 8** appellent `\slidetitlepage`.
`chapitre1` et `chapitre4` la recomposent entièrement, logo en dur
(`\includegraphics{Logo-toulouse-inp-N7.png}`) au lieu de `\ocotslogos` — si
l'établissement change, ces deux pages ne suivront pas. `chapitre1` a une
raison réelle (un QR code que `\slidetitlepage` ne sait pas accueillir) ;
`chapitre4` copie ce contournement sans en avoir besoin.

> **En attendant la révision du template.** L'absence de point d'extension
> dans `\slidetitlepage` est demandée au
> [chantier 7](template.md#chantier-7--un-hook-dans-slidetitlepage).
