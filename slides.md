<!-- LTeX: language=fr-FR -->

# Règles de rédaction — transparents

Support `beamer` + `\usepackage{ocots}` (le support est déduit de la classe).
Prérequis : [`communes.md`](communes.md).

> **Squelette.** Ces règles sont posées d'après la pratique existante mais
> n'ont pas encore été éprouvées par une passe de relecture complète. À affiner.

Numérotation stable : `SL1`, `SL2`, … (voir [`README.md`](README.md#numérotation--cest-une-api)).

---

## SL1 — Le polycopié est la référence, dans un seul sens

**Tout ce qui est dans les transparents doit se retrouver dans le polycopié.
L'inverse n'importe pas** — le poly peut être plus riche, les transparents sont
une sélection.

C'est la règle qui structure les passes d'harmonisation : on inventorie ce qui
est en slide et absent du poly, et on le remonte dans le poly (jamais l'inverse).
Les notations et le vocabulaire suivent le poly (règle
[C2](communes.md#c2--cohérence-terminologique-et-notationnelle)).

## SL2 — Une idée par diapositive

Une diapositive porte **un** énoncé, **un** exemple, **une** étape de
raisonnement. Deux idées → deux diapositives. Un `slide` qui déborde est un
`slide` à couper, pas à réduire en corps 7.

## SL3 — Le transparent n'est pas le polycopié

Le texte d'un transparent est **télégraphique** ; la narration est **orale**.
Conséquence : les règles d'amorce et de reprise du polycopié
([poly 3](poly.md#3-une-phrase-damorce-motivée-avant-chaque-boîte) et
[poly 4](poly.md#4-une-phrase-de-reprise-après-la-boîte)) **ne s'appliquent
pas** ici — c'est l'enseignant qui fait la liaison.

En revanche, la règle
[poly 2](poly.md#2-pas-de-blocs-isolés-ou-enchaînés-sans-texte) survit sous une
autre forme : **une suite de boîtes sans titre ni fil visible** reste illisible
en projection. Le fil passe par les titres de diapositives.

## SL4 — Pas de preuve longue en transparent

Une preuve de plus d'une diapositive : garder **l'idée** et renvoyer au
polycopié. Les preuves fractionnées existent quand c'est vraiment nécessaire
(`proofbegin` / `proofmiddle` / `proofend`), mais c'est l'exception.

## SL5 — `\pause` : progression, pas décoration

On découvre progressivement ce qui doit être **commenté** au fur et à mesure, ou
ce dont la surprise sert (un contre-exemple, un résultat inattendu). On ne met
pas de `\pause` sur une liste que l'on lit intégralement.

La remise à zéro des compteurs entre deux `\pause` est gérée par le support du
template — ne pas la bricoler dans le document.

## SL6 — Figures lisibles en projection

Une figure de transparent n'est pas une figure de polycopié rétrécie :
épaisseurs de trait, taille des étiquettes et contraste se règlent **pour la
projection**. Vérifier en plein écran, pas dans l'aperçu.

Les figures des transparents vivent dans le dossier du chapitre
(`slides/chapitreN/figures/`) — une figure partagée avec le poly se **copie**,
elle ne se référence pas par un chemin relatif qui traverse les dossiers.

## SL7 — Le thème est un réglage global

`theme=ocots` par défaut ; `legacy-dark` reste disponible pour la projection.
Le choix se fait **au préambule**, une fois. Pas de couleur posée à la main dans
le corps d'une diapositive (`\slidecolor` existe pour changer la couleur
d'en-tête d'une série).
