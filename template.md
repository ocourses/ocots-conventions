<!-- LTeX: language=fr-FR -->

# Révisions demandées au template

Ce que la relecture des conventions a mis au jour dans
[`ocots-latex-template`](https://github.com/ocourses/ocots-latex-template), et
qui doit y être corrigé pour que les règles soient applicables.

> **État : proposition.** **Rien n'est encore appliqué au template.** Ce
> document est la spec des PR à y faire. Les cours migreront ensuite, à leur
> rythme, chacun à sa version épinglée du sous-module.

| Chantier | Objet | Règle concernée |
|---|---|---|
| **1** | [Signature des environnements et labels](#chantier-1--signature-des-environnements-et-labels) | [C5](communes.md#c5--labels-et-renvois) |
| **2** | [Nommage des macros mathématiques](#chantier-2--nommage-des-macros-mathématiques) | [C3](communes.md#c3--macros-du-template-plutôt-que-du-latex-manuel) |
| **3** | [Correctifs divers](#chantier-3--correctifs-divers) | [C1](communes.md#c1--langue-et-registre), [C4](communes.md#c4--typographie) |

Chaque chantier se vérifie par `cd examples && make`, qui **doit rester vert**.

---

# Chantier 1 — Signature des environnements et labels

## Le problème

Trois mécanismes d'étiquetage coexistent, avec des comportements différents :

| Environnements | Mécanisme | Où va la clé | Label produit | Compteur |
|---|---|---|---|---|
| `theorem` `definition` `proposition` `corollary` `conjecture` | `\newtcbtheorem` (`ocots-env.sty:41-50`) | 2ᵉ argument obligatoire | **préfixé** : `thm:clé`, `def:clé`… | **partagé** |
| `lemma` `example` `remark` | `\newtheorem` (`ocots-env.sty:63-81`) | `\label{}` dans le corps | brut | **un chacun** |
| `exercise` | `tcolorbox`, clé `label=` | clé `label=` | brut | propre |

Quatre conséquences fâcheuses :

- **Le double préfixe.** Écrire `\begin{theorem}{}{thm:cauchy}` donne
  `thm:thm:cauchy` et casse le renvoi. C'est l'erreur qu'a dû réparer la salve 4
  de la relecture d'`automatique-enseignants`.
- **Les arguments obligatoires vides.** `\begin{theorem}{}{}` est la forme la
  plus fréquente dans le corpus : deux accolades vides que l'auteur doit taper
  sans raison.
- **La doc ment.** `doc/commandes.md:117` annonce que `label=<nom>` pose
  `\label{ex:<nom>}`. Faux : la clé est brute. Le préfixe `ex:` n'existe que
  dans `ocots-compat.sty:50`, pour l'ancienne macro `\myexercisecb`.
- **Un numéro ne désigne pas un objet.** Le commentaire du template annonce
  qu'« un théorème et une définition ne portent jamais le même numéro ». C'est
  vrai de la première famille seulement : `lemma`, `example` et `remark` ont
  chacun leur compteur, et les séquences s'entrelacent.

  ```text
  Définition 2.1.1 – Tribu
  Exemple 2.1.1. Les familles suivantes sont des tribus…
  Remarque 2.1.1. Les éléments de la tribu sont appelés…
  Définition 2.1.2 – Espace mesurable
  ```

  Relevé dans les PDF compilés : **56 numéros sur 91 désignent plus d'un objet**
  dans `mesure-integration`, **44 sur 82** dans `calcul-differentiel-edo`. Ce
  n'est pas un choix éditorial, c'est la conséquence d'avoir deux mécanismes.

## La cible

**Une seule signature, une seule règle** : tout environnement prend une liste
clé-valeur optionnelle, et `label=` pose le label **tel quel**.

```latex
\begin{theorem}[title={Théorème de Cauchy-Lipschitz}, label=thm:cauchy]
\begin{proposition}[label=prop:duhamel]
\begin{remark}[label=rem:autonome]
\begin{example}[note={Contre-exemple}, label=exa:non-unicite]
\begin{exercise}[label=ex:matrices, points=4]
\begin{definition}                                  % ni titre ni label
```

Clés communes : `title=`, `label=`, `note=`. Clés propres à `exercise` :
`points=`, `nosolution`.

**Et un seul compteur.** `theorem`, `definition`, `proposition`, `corollary`,
`conjecture`, `lemma`, `example`, `remark` partagent la même séquence, par
section — décidé avec l'auteur. Un numéro désigne alors un objet, sans
exception : une remarque garde son numéro (elle n'est pas dégradée en
`remark*`), c'est le compteur qui cesse d'être privé.

```latex
\newtcbtheorem[number within=section]%
  {theorem}{\ocotsstring{theorem}}{\ocotsboxstyle{theorem}}{thm}
\newtcbtheorem[use counter from=theorem]%
  {definition}{\ocotsstring{definition}}{\ocotsboxstyle{definition}}{def}
\newtheorem{lemma}[theorem]{\ocotsstring{lemma}}
\newtheorem{example}[theorem]{\ocotsstring{example}}
\newtheorem{remark}[theorem]{\ocotsstring{remark}}
```

`[theorem]` remplace `[section]` : le compteur suit celui de `theorem` au lieu
d'en ouvrir un à lui, exactement le mécanisme que `\newtcbtheorem` utilise déjà
entre `definition` et `theorem` (`use counter from=theorem`). `exercise`,
`assumption`, `openquestion`, `difficulty` gardent leur séquence propre : ce
sont des objets d'une autre nature (H1, Q1, D1 sont déjà un espace de noms à
part), pas des résultats numérotés dans le même fil.

Ce que ça règle :

- **plus d'arguments obligatoires vides** — on n'écrit que ce qu'on a ;
- **plus de double préfixe** — impossible par construction ;
- **un seul mécanisme** au lieu de trois ;
- **la doc redevient vraie**, et `ocots-compat.sty:50` n'a plus à inventer `ex:` ;
- **un numéro désigne un objet** — les 56 ambiguïtés relevées dans
  `mesure-integration` disparaissent, et la numérotation devient comparable
  entre poly et transparents ([SL2](slides.md#sl2--viser-une-numérotation-identique-au-polycopié)).

## Points techniques vérifiés

- Le deux-points de `\newtcbtheorem` est **codé en dur** : un préfixe vide
  produit `:clé`, pas `clé`. Il faut donc abandonner le mécanisme de label de
  `\newtcbtheorem` au profit de la clé `label=` de `tcolorbox` — celle
  qu'`exercise` utilise déjà, et qui pose un label brut.
- `cleveref` aurait aussi supprimé la règle des renvois capitalisés de
  [C4](communes.md#c4--typographie) (`Théorème~\ref{…}`). **Il ne s'intègre pas**
  avec `\newtcbtheorem` — testé, 249 erreurs, y compris avec l'ordre de
  chargement documenté. À réessayer une fois le chantier 1 fait, puisque les
  boîtes ne passeront plus par `\newtcbtheorem`.

## Migration

`ocots-compat.sty` porte la forme `{titre}{clé}` en alias déprécié, avec le
préfixe historique, le temps que les cours migrent.

Le compteur partagé, lui, **ne se déprécie pas en douceur** : dès que le
template change, toute la numérotation du poly se décale à la recompilation —
« Exemple 2.1.1 » peut devenir « Exemple 2.1.4 ». Les `\ref` se recalculent
seuls ; ce qui casse en silence, ce sont les numéros écrits en dur hors du
poly. Le corpus en compte quatre, tous dans des TD
(`td/td1/td1.tex:93`, `td/td4/td4.tex:171,307,331`) — déjà fragiles, puisque
[TD5](td.md#td5--nommer-le-résultat-du-cours-mobilisé) demande de nommer un
résultat plutôt que de le numéroter. Une passe de migration les **grep** avant
de migrer :

```bash
grep -rnE '(Théorème|Définition|Proposition|Corollaire|Lemme|Exemple|Remarque)~?[ ]?[0-9]+\.[0-9]+' \
  --include='*.tex' td/ slides/ exam/
```

et remplace le numéro par un `\ref` ou par le nom du résultat.

---

# Chantier 2 — Nommage des macros mathématiques

Spec de refonte des modules `tex/math/`. Complète la règle
[C3](communes.md#c3--macros-du-template-plutôt-que-du-latex-manuel), qui dit
d'utiliser les macros ; cette partie dit **comment elles s'appellent**.

## Les six principes de nommage

> Numérotés `N*` — `P*` est réservé aux règles du
> [polycopié](poly.md) (voir
> [`README.md`](README.md#citer-une-règle--épingler-la-version)).

**N1 — Le nom est en anglais, la sortie suit `lang=`.**
Le mécanisme existe (`\ocotsstring{op-…}`) mais seuls trois opérateurs s'en
servent aujourd'hui. Il doit devenir la norme pour tout opérateur dont le rendu
diffère d'une langue à l'autre : `\rank` affiche « rg » en français, « rank » en
anglais ; `\sinh` affiche « sh » en français, « sinh » en anglais.

**N2 — Un concept, une macro.**
Aujourd'hui la lettre E a deux macros (`\E` en `\mathcal`, `\Ecal` en
`\mathscr`) et la lettre F en a trois (`\F`, `\Fcal`, `\FT`). Une seule doit
survivre par concept.

**N3 — Jamais un nom d'une lettre, jamais la redéfinition d'une macro standard.**
`\O` (Ø) et `\P` (¶) sont aujourd'hui écrasées silencieusement. `\B \C \D \E \F
\K \M \U \X` monopolisent des noms d'une lettre. Seule exception assumée : les
ensembles de nombres `\R \N \Z \Q \C \K`, dont l'usage est universel.

**N4 — Le nom dit le concept, pas le glyphe.**
`\Lcal` est nommée d'après son dessin. `\Borel` se lit, `\Bor` se devine.

**N5 — Lisible jusqu'au bout, plutôt que concis.**
En cas d'arbitrage, le nom long gagne. `\ContinuousLinear` plutôt que `\Lcal`,
`\PowerSet` plutôt que `\Parties`.

**N6 — La police est une option, pas un nom.**
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
| `\N \Z \Q \R \C \K` | ensembles usuels | **inchangés** (exception N3) |
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
| `\B \D \E \F \O \P \U \X \M` | lettres nues | **à supprimer** (N3) |
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
| `\rang` + `\rank` | « rg » / « rank » | **`\rank`**, localisé | doublon (N1) |
| `\im` | Im | inchangé | — |
| `\Ker` | Ker | inchangé | — |
| `\vect` | Vect | `\spanop`, localisé | N1 |
| `\comatrice` | com | `\cofactor`, localisé | N1 |
| `\graphe` | graphe | `\graph`, localisé | N1 |
| `\sh \argsh` | sh, argsh | `\sinh \argsinh` **redéfinis**, localisés | N1 |
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

---

# Chantier 3 — Correctifs divers

Défauts relevés en passant, indépendants des deux chantiers précédents.

| Fichier | Défaut | Correctif |
|---|---|---|
| `ocots-text.sty:38-39` | `\ie` et `\cf` n'ont pas `\xspace` : `\ie foo` donne « i.e.foo ». C'est ce qui oblige à écrire `\ie~` partout dans les cours | ajouter `\xspace` — le paquet est déjà chargé |
| `ocots-packages.sty` | rien ne fournit de guillemets suivant la langue, d'où `` ``…'' `` — des guillemets **anglais** — dans les trois cours français | charger `csquotes` avec `autostyle=true`, documenter `\enquote{…}` ([C4](communes.md#c4--typographie)) |
| `ocots-text.sty:38-39` | `\ie` et `\cf` sont figés, alors que d'autres chaînes passent par `\ocotsstring` | les localiser (principe N1) |
| `doc/commandes.md:117` | annonce que `label=<nom>` pose `\label{ex:<nom>}` — faux, la clé est brute | corrigé par le chantier 1 ; en attendant, aligner la doc sur le code |
| `ocots-env.sty` | **rien pour l'introduction de chapitre**, alors que la règle [P7](poly.md#p7--ouverture-de-chapitre-et-de-section) en demande une, composée en retrait. Les cours détournent `quote` ou `quotation` — des environnements de *citation* — et le corpus est incohérent : `controle_optimal` emploie les deux, dans le même polycopié | ajouter un environnement `chapterintro` : le retrait voulu, un nom qui dit ce que c'est, et un rendu réglable par le thème |

`\enquote` est vérifié : sous `[french]{babel}` avec `autostyle`, il rend
« ceci » en français et “this” en anglais, et gère l'imbrication
(« a “b” » / “a ‘b’ ”).
