<!-- LTeX: language=fr-FR -->

# `ocots-conventions` — conventions de rédaction des cours

Comment on **rédige** un support de cours — polycopié, transparents, TD, sujet
d'examen. C'est le pendant de
[`ocots-latex-template`](https://github.com/ocourses/ocots-latex-template), qui
dit comment on le **compose**.

| Dépôt | Question à laquelle il répond |
|---|---|
| `ocots-latex-template` | quelle commande, quel environnement, quel rendu ? |
| `ocots-conventions` (ici) | quel texte autour, dans quel ordre, avec quelles notations ? |

Les deux couvrent les **mêmes quatre supports**. Un cours les monte tous les
deux en sous-module et **épingle une version de chacun** — les conventions
citent des macros du template (`\keyword`, `\index`, `\norm`, `\diff`), les deux
versions doivent rester cohérentes.

---

## À lire dans cet ordre

1. **[`communes.md`](communes.md)** — le socle : ce qui vaut sur les quatre
   supports (langue, notations, macros, typographie, labels, listes).
   **À lire dans tous les cas.**
2. Le fichier du support sur lequel on travaille :
   - **[`poly.md`](poly.md)** — polycopié (`P1`–`P14`)
   - **[`slides.md`](slides.md)** — transparents
   - **[`td.md`](td.md)** — TD et corrigés
   - **[`exam.md`](exam.md)** — sujets d'examen
3. **[`methode.md`](methode.md)** — comment se déroule une passe de relecture :
   salves, périmètre, compilation, PR, format du rapport.

En annexe, **[`template.md`](template.md)** — les révisions demandées au
template LaTeX. Proposition, rien n'est encore appliqué.

Le fil conducteur, si on ne devait retenir qu'une phrase :

> **On n'enchaîne jamais des boîtes sans texte autour.** Chaque boîte est amenée
> par une phrase qui dit *pourquoi elle arrive*, et suivie d'une phrase qui
> *exploite* ce qu'elle apporte. Le lecteur ne doit jamais tomber sur un énoncé
> sans savoir ce qu'on cherche à en faire.

---

## Installation dans un cours

```bash
git submodule add git@github.com:ocourses/ocots-conventions.git conventions
git commit -m "chore: ajoute le sous-module conventions"
```

Le dépôt de cours obtient alors la paire :

```text
template/       ← comment ça se compose   (ocots-latex-template)
conventions/    ← comment ça se rédige    (ocots-conventions)
```

**Rien à configurer** : pas de `TEXINPUTS`, pas d'impact sur `latexmk` ni sur
la CI LaTeX. Seuls les outils de `bin/` demandent
[`uv`](https://docs.astral.sh/uv/) (voir [Vérifier](#vérifier)).

Mise à jour : épingler un tag, `git -C conventions checkout vX.Y.Z`, puis un
commit du pointeur (voir [Versions](#versions)).

### Pour un agent `ocourses/agents`

Aucune modification d'infra : l'étape « Sous-modules privés » du workflow
réutilisable initialise **tous** les sous-modules déclarés dans `.gitmodules`.
L'agent trouve donc `conventions/` sur disque, à la version épinglée par ce
cours. La résolution (`conventions/` du cours, sinon clone par le script) est
faite par la couche mécanique, pas par l'agent — il n'a ni `webfetch` ni
`websearch`.

---

## Vérifier

```bash
./conventions/bin/verifier            # toutes les règles mécaniques, sur le dépôt
./conventions/bin/verifier P2 poly/   # une règle, un périmètre
./conventions/bin/verifier --list     # ce qui est implémenté
./conventions/bin/verifier --mesure   # compte des motifs non bloquants
```

Sortie `1` s'il y a au moins une infraction — utilisable en CI.

**`bin/verifier` et `bin/nettoyer` sont des relais** vers
[`ocots-lint`](https://github.com/ocourses/ocots-lint), où l'outil est
développé, testé et publié. Chaque version des conventions épingle la version
d'`ocots-lint` qui connaît ses règles. Prérequis : `uv` ; sans lui, sortie `2`.

`ocots-lint` va plus loin que ces relais : garantie déclarée par règle
(`ocots-lint couverture`), exemptions justifiées dans la source
(`% ocots-lint: ignore P5 — raison`), sorties pour la CI
(`--format github|json|sarif`), et ses limites connues écrites comme tests.
Voir son [README](https://github.com/ocourses/ocots-lint#readme).

**Seules les règles *mécaniques* sont outillées**, celles qui se tranchent sans
jugement. Une règle qui demande de l'interprétation
([C2](communes.md#c2--cohérence-terminologique-et-notationnelle),
[P1](poly.md#p1--placement-des-hypothèses),
[P3](poly.md#p3--une-phrase-damorce-motivée-avant-chaque-boîte)…) porte à la
place une commande `grep` dans sa section : elle **mesure** l'ampleur, elle ne
décide pas.

| Règle | Ce qui est vérifié |
|---|---|
| `P2` | enchaînements de boîtes sans texte entre elles (série d'exercices et entrée en `remark` tolérées) |
| `P3` | amorces passe-partout, phrases qui se jettent dans la boîte (signaux, pas verdicts) |
| `P5` | plus de trois `remark` d'affilée — un signal, pas une faute |
| `C4` | `~:` inutiles, guillemets, apostrophes U+2019, renvois en bas de casse, mots composés |
| `C6` | preuve ou exemple fini par une liste ou une équation sans `\qedhere` ; `\vspace` collé à un `\footnotetext` |

Certains motifs sont sûrs mais encore trop répandus pour bloquer : ancienne
syntaxe `{titre}{label}` des boîtes et `\emph{\textbf{…}}` (C3), étapes
numérotées à la main « i) » au lieu de `\newstep` (C4), « t.q. » (C1), espaces
verticaux manuels (C6). `--mesure` les **compte** sans jamais échouer, pour
suivre leur baisse d'une relecture à l'autre ; une mesure deviendra une règle
quand le corpus sera propre.

```bash
./conventions/bin/verifier --mesure poly/ 2>&1 >/dev/null   # les comptes seuls
```

Les contrôles typographiques **masquent le mode mathématique** avant de
chercher : `~` y est une espace, et `\forall h \in E ~:~ J'(x) \cdot h = 0` ne
doit pas être « corrigé ».

`P3` **ne s'applique pas** aux fichiers d'un dossier `slides/` :
[`slides.md#sl4`](slides.md#sl4--le-transparent-nest-pas-le-polycopié) exempte
explicitement le support transparent des règles d'amorce du polycopié — le
texte y est télégraphique, c'est l'enseignant qui fait la liaison à l'oral.

### Ce que l'outil ne fait pas

**`verifier` aide à l'analyse ; il ne certifie rien.**

- Il **rate des choses.** `P2` ne voit que des boîtes séparées par du blanc :
  une ligne de `%` ou un `\medskip` entre deux boîtes suffit à la lui cacher,
  alors que l'infraction est la même. `P3` ne juge pas si une amorce est
  *motivée* — il ne repère que deux formes sûres.
- Il **signale du correct.** Fermer une section sur un théorème
  ([P4](poly.md#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé)) est
  souvent légitime, et une amorce qu'il pointe peut être la bonne. Quatre
  remarques d'affilée ([P5](poly.md#p5--ce-quest-une-remark)) peuvent être
  quatre vrais apartés — l'outil dit *où regarder*, pas *quoi corriger*.
- **Zéro trouvaille ne veut pas dire règle respectée.** Les règles qui portent
  le plus — [P1](poly.md#p1--placement-des-hypothèses),
  [P4](poly.md#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé),
  [P7](poly.md#p7--ouverture-de-chapitre-et-de-section) — ne sont pas
  outillables du tout.

Ce à quoi il sert vraiment : **mesurer une ampleur** avant une passe et
**suivre une baisse** après. Pas remplacer la lecture.

## Corriger

`verifier` **analyse** ; `nettoyer` **modifie**.

```bash
./conventions/bin/nettoyer C4 poly/              # aperçu, rien n'est écrit
./conventions/bin/nettoyer C4 poly/ --appliquer  # écrit les fichiers
```

Il n'automatise que les corrections dont **l'équivalence a été vérifiée**, et
refuse celles dont le document n'a pas les moyens : sans `csquotes` chargé,
réécrire les guillemets en `\enquote{…}` rendrait le document incompilable, donc
il ne le fait pas et le dit.

Éprouvé sur le polycopié de mesure : **183 `~:` retirés, compilation `exit=0`,
warnings inchangés, et le texte du PDF identique caractère pour caractère.**

Après application : **recompiler, relire le diff, et commiter à part.** Un
nettoyage mécanique est sa propre salve — il ne se mêle pas à une passe de fond.

---

## Citer une règle : épingler la version

Les journaux de relecture et les messages de commit citent les règles par leur
identifiant. Plutôt que de **figer** la numérotation pour toujours, on
**épingle la version** des conventions appliquée — ce qui laisse la liberté de
réordonner, fusionner ou scinder les règles.

### Les identifiants

| Fichier | Espace |
|---|---|
| `communes.md` | `C1`, `C2`, … |
| `poly.md` | `P1`, `P2`, … |
| `slides.md` | `SL1`, `SL2`, … |
| `td.md` | `TD1`, `TD2`, … |
| `exam.md` | `EX1`, `EX2`, … |

Un identifiant **préfixé** (`P5`, `C2`) dit à lui seul qu'il désigne ces
conventions-ci. Un **nombre nu** (« règle 5 ») est une citation antérieure à ce
dépôt : voir [`CHANGELOG.md`](CHANGELOG.md) pour la correspondance.

### Toute passe de relecture épingle sa version

Le fichier de suivi (`reports/<passe>/00-suivi.md`, ou le fichier de run d'un
agent) porte, en tête :

```text
Conventions : ocots-conventions v2.1.0 (commit 1a2b3c4)
```

```bash
git -C conventions describe --tags --always
```

C'est ce qui rend une trace de relecture relisable des années plus tard : on
sait quelles règles étaient en vigueur, et on peut les retrouver exactement.
Le pointeur de sous-module l'enregistre déjà dans l'historique du cours ;
l'écrire dans le suivi le rend lisible sans fouiller le `git log`.

### Versions

Les conventions suivent [SemVer](https://semver.org/lang/fr/). L'interface
publique, ce sont les **identifiants de règle** : ce qu'une trace de relecture
cite.

| | Ce qui la fait monter |
|---|---|
| **majeur** | un identifiant change de sens : règle renumérotée, fusionnée, scindée, retirée |
| **mineur** | règle ajoutée, durcie ou assouplie ; identifiants inchangés |
| **correctif** | reformulation, exemples, cas concrets, coquilles |

Tout changement majeur porte sa **table de correspondance** dans
[`CHANGELOG.md`](CHANGELOG.md), pour qu'une trace ancienne reste lisible.

Publier une version :

1. la section « Non publié » du CHANGELOG devient `## vX.Y.Z — date` ;
2. `git tag -a vX.Y.Z -m "…"` puis `git push origin vX.Y.Z` ;
3. la release GitHub est créée par la CI, avec la section du CHANGELOG pour
   notes. Elle échoue si la section manque.

---

## Ce qui reste dans le dépôt de cours

Ce dépôt porte les **règles**. Restent propres à chaque cours, dans
`reports/<passe>/06-regles-style-relecture.md` :

- le **décor de section** (règle [P7](poly.md#p7--ouverture-de-chapitre-et-de-section)) — la liste
  des objets courants du cours :
  $x, u, y, f, g$ en automatique ; $E, F, U, x$ en calcul différentiel ;
  $(X, \mathcal{A}, \mu)$ en mesure et intégration ;
- les **cas concrets repérés** dans les fichiers du cours (`fichier:ligne`) ;
- les **arbitrages de terminologie** propres au cours (« point d'équilibre »
  plutôt que « point de fonctionnement », « Lyapunov » plutôt que
  « Liapounov »…).

---

## État

| Fichier | État |
|---|---|
| `communes.md` | `C1`–`C6` — relues avec l’auteur |
| `poly.md` | `P1`–`P14` — toutes relues avec l’auteur, éprouvées sur deux cours |
| `methode.md` | v1 |
| `td.md` | `TD1`–`TD9` — toutes relues avec l’auteur |
| `slides.md` | `SL1`–`SL10` — toutes relues avec l’auteur |
| `exam.md` | `EX1`–`EX7` — toutes relues avec l’auteur |
| `template.md` | 7 chantiers — **non appliqués** au template |

---

## Origine

Les 13 règles du polycopié ont été dégagées pendant la relecture ligne à ligne
de `ocourses/calcul-differentiel-edo-enseignants`
(`reports/poly-update-2026/06-regles-style-relecture.md`), puis reprises et
adaptées aux noms d'environnements `ocots` sur
`ocourses/automatique-enseignants`
(`reports/poly-relecture-2026/06-regles-style-relecture.md`). Ce dépôt met fin à
la duplication : les règles vivent ici, les cours n'en gardent que les
adaptations.
