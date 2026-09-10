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
   - **[`poly.md`](poly.md)** — polycopié (les 13 règles)
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

**Rien à configurer** : ce dépôt ne contient que du Markdown. Pas de
`TEXINPUTS`, pas d'impact sur `latexmk` ni sur la CI LaTeX.

Mise à jour : `git -C conventions pull` puis un commit du pointeur.

### Pour un agent `ocourses/agents`

Aucune modification d'infra : l'étape « Sous-modules privés » du workflow
réutilisable initialise **tous** les sous-modules déclarés dans `.gitmodules`.
L'agent trouve donc `conventions/` sur disque, à la version épinglée par ce
cours. La résolution (`conventions/` du cours, sinon clone par le script) est
faite par la couche mécanique, pas par l'agent — il n'a ni `webfetch` ni
`websearch`.

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
Conventions : ocots-conventions v1.0.0 (commit 1a2b3c4)
```

```bash
git -C conventions describe --tags --always
```

C'est ce qui rend une trace de relecture relisable des années plus tard : on
sait quelles règles étaient en vigueur, et on peut les retrouver exactement.
Le pointeur de sous-module l'enregistre déjà dans l'historique du cours ;
l'écrire dans le suivi le rend lisible sans fouiller le `git log`.

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
| `communes.md` | `C1`–`C6` — relues avec l'auteur jusqu'à `C4` |
| `poly.md` | `P1`–`P14` — éprouvées sur deux cours |
| `methode.md` | v1 |
| `td.md` | `TD1`–`TD9` — dérivées du rôle `exercise-corrector` de `ocourses/agents` |
| `slides.md` | `SL1`–`SL7` — **squelette**, à affiner |
| `exam.md` | `EX1`–`EX7` — **squelette**, à affiner |
| `template.md` | 3 chantiers — **non appliqués** au template |

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
