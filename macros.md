<!-- LTeX: language=fr-FR -->

# Nommage des macros mathématiques

Spec de refonte des modules `tex/math/` de
[`ocots-latex-template`](https://github.com/ocourses/ocots-latex-template).
Complète la règle [C3](communes.md#c3--macros-du-template-plutôt-que-du-latex-manuel),
qui dit d'utiliser les macros ; ce document dit **comment elles s'appellent**.

> **État : proposition.** Rien n'est encore appliqué au template. La migration
> passera par `ocots-compat.sty`, qui porte déjà les alias dépréciés de la v0.

---

## Les six principes

**P1 — Le nom est en anglais, la sortie suit `lang=`.**
Le mécanisme existe (`\ocotsstring{op-…}`) mais seuls trois opérateurs s'en
servent aujourd'hui. Il doit devenir la norme pour tout opérateur dont le rendu
diffère d'une langue à l'autre : `\rank` affiche « rg » en français, « rank » en
anglais ; `\sinh` affiche « sh » en français, « sinh » en anglais.

**P2 — Un concept, une macro.**
Aujourd'hui la lettre E a deux macros (`\E` en `\mathcal`, `\Ecal` en
`\mathscr`) et la lettre F en a trois (`\F`, `\Fcal`, `\FT`). Une seule doit
survivre par concept.

**P3 — Jamais un nom d'une lettre, jamais la redéfinition d'une macro standard.**
`\O` (Ø) et `\P` (¶) sont aujourd'hui écrasées silencieusement. `\B \C \D \E \F
\K \M \U \X` monopolisent des noms d'une lettre. Seule exception assumée : les
ensembles de nombres `\R \N \Z \Q \C \K`, dont l'usage est universel.

**P4 — Le nom dit le concept, pas le glyphe.**
`\Lcal` est nommée d'après son dessin. `\Borel` se lit, `\Bor` se devine.

**P5 — Lisible jusqu'au bout, plutôt que concis.**
En cas d'arbitrage, le nom long gagne. `\ContinuousLinear` plutôt que `\Lcal`,
`\PowerSet` plutôt que `\Parties`.

**P6 — La police est une option, pas un nom.**
Un auteur choisit une fois, au préambule, la fonte de ses ensembles ; il ne la
réécrit pas à chaque appel. C'est la philosophie du template — le thème fournit
l'apparence, l'environnement ne la connaît jamais.

---

## Deux nouvelles options

| Option | Valeurs | Défaut | Effet |
|---|---|---|---|
| `setfont` | `bb`, `rm` | `bb` | `\R` → $\mathbb{R}$ ou $\mathrm{R}$ |
| `calfont` | `scr`, `cal` | `scr` | familles calligraphiques → `\mathscr` ou `\mathcal` |

`setfont` est presque gratuit : `base` a déjà `\nbSet{#1}` comme unique point
d'indirection des ensembles de nombres. `calfont` demande la même indirection
pour les lettres calligraphiques — c'est le principal travail de la refonte.

---

## `base` — ensembles de nombres

| Actuel | Sens | Proposé |
|---|---|---|
| `\N \Z \Q \R \C \K` | ensembles usuels | **inchangés** (exception P3) |
| `\Sn` | sphère $\mathbb{S}$ | `\Sphere` |
| `\Rp` | $[0,+\infty)$ | `\Rnonneg` |
| `\Rsp` | $(0,+\infty)$ | `\Rpos` |
| `\Rn` | $(-\infty,0]$ | `\Rnonpos` |
| `\Rsn` | $(-\infty,0)$ | `\Rneg` |
| `\Rs` | $\R\setminus\{0\}$ | `\Rstar` |
| `\Rb` | droite achevée $\overline{\R}$ | `\Rbar` |
| `\Rbp` | $[0,+\infty]$ | `\Rbarnonneg` |
| `\Nb \Ns \Nsb` | idem sur $\N$ | `\Nbar \Nstar \Nbarstar` |
| `\nbSet \setDeco \setPositive \setNegative \setStar` | plomberie | internes, préfixe `\ocots@` |

Le gain : `\Rsp` ne se devine pas, `\Rpos` se lit. La distinction
positif / positif ou nul, aujourd'hui portée par un `s` muet, devient explicite.

## `base` — familles calligraphiques

C'est ici que se joue la fusion des trois familles concurrentes. Toutes passent
par une indirection unique pilotée par `calfont`, et **prennent un nom de
concept** quand elles en ont un.

| Actuel | Sens | Proposé |
|---|---|---|
| `\Vcal` | voisinages de $x$ | `\Neighborhoods` |
| `\Ical` | intervalle de temps | `\TimeInterval` |
| `\Lcal` | applications linéaires continues | `\ContinuousLinear` |
| `\Ccal` + `\xCn{k}` | applications de classe $C^k$ | `\Cclass{k}` |
| `\Fcal` | toutes les applications | `\AllMaps` |
| `\Ecal` | espaces $\Ecal_k$ des $k$-linéaires | `\MultilinearSpace` |
| `\GLcal \GL` | automorphismes / groupe linéaire | `\GL` seul |
| `\Acal` | ensemble atteignable $\Acal(t,x_0)$ | `\Reachable` |
| `\Dcal \Kcal \Ncal \Ucal \Htrue` | **aucun emploi** dans les trois cours | **à supprimer** |
| `\B \D \E \F \O \P \U \X \M` | lettres nues | **à supprimer** (P3) |
| `\Sgot` | groupe symétrique, **aucun emploi** | à supprimer, ou `\SymGroup` s'il ressert |
| `\ind` | indicatrice $\mathds{1}$ | `\indicator` |

`\O` et `\P` retrouvent leur sens LaTeX standard (Ø et ¶).

## `base` — délimiteurs et constructions

| Actuel | Sens | Proposé |
|---|---|---|
| `\abs` + `\absStyle` | valeur absolue, version `\left\right` | `\abs{}` et `\abs*{}` |
| `\norm` + `\normStyle` | norme, idem | `\norm{}` et `\norm*{}` |
| `\prodscal` | produit scalaire | `\inner` |
| `\enstq{x}{P}` | $\{x \mid P\}$ | `\setst` |
| `\intervalleff{a}{b}` | $[a,b]$ | `\intervalcc` |
| `\intervalleof{a}{b}` | $(a,b]$ | `\intervaloc` |
| `\intervallefo{a}{b}` | $[a,b)$ | `\intervalco` |
| `\intervalleoo{a}{b}` | $(a,b)$ | `\intervaloo` |
| `\intervalleentier` | $\llbracket p,q\rrbracket$ | `\intervalint` |
| `\fonction` | tableau de définition à 5 arguments | `\functiondef` |
| `\semidefpos \defpos \defneg` | $\succeq 0$, $\succ 0$, $\prec 0$ | `\possemidef \posdef \negdef` |
| `\veps \vphi` | `\varepsilon`, `\varphi` | **inchangés** (usage universel) |

Les paires `\abs`/`\absStyle` et `\norm`/`\normStyle` disparaissent au profit de
`\DeclarePairedDelimiter` (mathtools, déjà chargé) : la variante étoilée donne
la version auto-dimensionnée. Deux macros de moins, et c'est l'idiome standard.

Attention à l'ordre en traduisant les intervalles : `of` (ouvert-fermé) devient
`oc` (open-closed), `fo` devient `co`.

## `base` — opérateurs

| Actuel | Rendu | Proposé | Motif |
|---|---|---|---|
| `\rang` + `\rank` | « rg » / « rank » | **`\rank`**, localisé | doublon (P1) |
| `\im` | Im | inchangé | — |
| `\Ker` | Ker | inchangé | — |
| `\vect` | Vect | `\spanop`, localisé | P1 |
| `\comatrice` | com | `\cofactor`, localisé | P1 |
| `\graphe` | graphe | `\graph`, localisé | P1 |
| `\sh \argsh` | sh, argsh | `\sinh \argsinh` **redéfinis**, localisés | P1 |
| `\trace \diag \card \supp \sign \codim \id \Hom \GL \Isom \sym \argmax \minimize` | — | inchangés | déjà anglais |
| `\inv` | inv | inchangé | application d'inversion sur $\Isom_c$ — emploi réel, nom correct |
| `\comp` | **num** | **à supprimer** | nom et rendu incohérents, aucun emploi |
| `\rand` | rand | **à supprimer** | aucun emploi |
| `\dd` | d | fusionner avec `\dif` d'`analysis` | doublon inter-modules |

`\sinh` et `\argsinh` méritent un mot : le nom doit être l'anglais standard, le
**rendu** reste la notation française (sh, argsh) via `\ocotsstring`. Le template
fournit déjà `\RedeclareMathOperator` pour écraser proprement l'opérateur LaTeX.

## `analysis`

| Actuel | Sens | Proposé |
|---|---|---|
| `\xdif \diff` | d droit des intégrales | **`\dif`** (un seul) |
| `\xDif \Diff` | D de Fréchet | **`\Dif`** (un seul) |
| `\pardiff` | ∂ | `\partialop` |
| `\frp{f}{x}` | $\partial f/\partial x$ | `\pd{f}{x}` |
| `\frpp \frpij` | dérivées secondes | `\pdd{f}{x}{y}` (un seul, mixte ou non) |
| `\frpxx \frptt \frpuu \frpxu \frpux` | secondes spécialisées | **à supprimer** — ce sont `\pdd{f}{x}{x}`, etc. |
| `\petito \grandO` | $o(\cdot)$, $O(\cdot)$ | `\smallo \bigO` |
| `\adherence` | adhérence | `\closure` |
| `\Ball \BallClosed` | boules | `\ball \closedball` |
| `\EnsembleQuotient` | quotient | `\quotient` |
| `\xLn{k}` | $\mathscr{L}^k$ | `\ContinuousLinear` avec exposant |
| `\convn` | `\rightarrow` | **à supprimer** |
| `\Lie` | crochet de Lie | inchangé |
| `\lsol \ssol \xsol \ysol \zsol \tsol` | $\bar{x}$… (contrôle optimal) | `\sol{x}`, et **déplacer dans `control`** |

Cinq macros de dérivées secondes spécialisées (`xx`, `tt`, `uu`, `xu`, `ux`)
disparaissent : ce sont des cas particuliers d'une macro à trois arguments.

## `measure`

| Actuel | Sens | Proposé |
|---|---|---|
| `\Bor` | tribu borélienne | `\Borel` |
| `\Parties` | ensemble des parties | `\PowerSet` |
| `\FM` | fonctions mesurables | `\Measurable` |
| `\FE` | fonctions étagées | `\Simple` |
| `\cl{x}` | classe d'équivalence $[x]$ | `\eqclass` |
| `\convps` | convergence p.p. | `\convae` |
| `\tribu` | alias de `\sigma` | **à supprimer** |
| `\AT \BT \NT \CT \OT \FT` | tribus $\mathcal{A}$, $\mathcal{B}$… | à nommer au cas par cas, ou lettres via `calfont` |

`\cl` est le pire nom du lot : en anglais on lit *closure*, alors qu'il désigne
une classe d'équivalence — l'inverse d'`\adherence`, qui elle est vraiment
l'adhérence.

## `control`

Le module mélange deux choses : des macros mathématiques et **onze noms de
logiciels** (`\bocop \controltoolbox \cotcot \hampath \lapack \minpack \nutopy
\tapenade \arc \expmap \hom`). Les noms de logiciels ne sont pas des maths : ils
relèvent d'un `\softwarename{…}` unique, ou d'un module à part.

---

## Migration

1. **PR sur `ocots-latex-template`** : nouveaux noms ajoutés, anciens conservés
   dans `ocots-compat.sty` avec avertissement de dépréciation. Rien ne casse.
2. Les cours migrent **à leur rythme**, chacun à sa version épinglée du
   sous-module.
3. Chaque ligne supprimée de `ocots-compat.sty` est une migration terminée —
   c'est déjà la doctrine du template pour les noms de la v0.
4. Les macros marquées **à supprimer** sortent sans alias : elles sont soit sans
   emploi, soit incorrectes.

## Emplois relevés

Comptage sur les trois cours (`automatique`, `calcul-differentiel-edo`,
`mesure-integration`), sous-module `template/` exclu :

```bash
grep -rn --include='*.tex' '\\Acal\b' . | grep -v '/template/' | wc -l
```

| Macro | Emplois | Verdict |
|---|---|---|
| `\Acal` | 29 | garder — c'est l'ensemble atteignable → `\Reachable` |
| `\inv` | 21 | garder — application d'inversion, nom déjà correct |
| `\ind` | 1 | garder → `\indicator` |
| `\Ncal \Ucal \Kcal` | 0 réel | supprimer (les occurrences trouvées sont dans une liste de macros, pas des emplois) |
| `\Dcal \Htrue \comp \rand \Sgot \Sn` | 0 | supprimer |

## Ce qu'il reste à trancher

`\AT \BT \CT \FT \NT \OT` (module `measure`) — faut-il des noms sémantiques,
sachant qu'une tribu porte rarement un nom fixe, ou un accès générique aux
lettres calligraphiques du type `\cal{A}` piloté par `calfont` ?
