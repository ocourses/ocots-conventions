<!-- LTeX: language=fr-FR -->

# Règles de rédaction — TD et corrigés

Classe `ocots-td`. Prérequis : [`communes.md`](communes.md).

Identifiants : `TD1`, `TD2`, … (voir [`README.md`](README.md#citer-une-règle--épingler-la-version)).

---

## TD1 — Le polycopié fait référence

Un TD **mobilise** le cours, il ne le refait pas. Notations, vocabulaire et
énoncés de résultats sont **identiques à ceux du polycopié** (règle
[C2](communes.md#c2--cohérence-terminologique-et-notationnelle)). Si l'exercice
attend une méthode vue en cours (exponentielle de matrice plutôt que
diagonalisation, Routh plutôt que valeurs propres…), c'est **celle du cours**
qu'on suit.

Une divergence énoncé / cours (notation, hypothèse) se **signale** en relecture,
elle ne se corrige pas en silence.

### Le signal : un macro-diff entre `td/` et `poly/`

Une macro utilisée dans les TD mais jamais dans le poly est un candidat direct :

```bash
comm -23 <(grep -rhoE '\\[A-Za-z]{2,}' --include='*.tex' td | sort -u) \
         <(grep -rhoE '\\[A-Za-z]{2,}' --include='*.tex' poly | sort -u)
```

Sur `mesure-integration`, ce diff remonte deux cas réels :

- **`\eqdef`** — redéfinie localement, en double (`td3.tex:7` et `td4.tex:9`,
  chacune avec son propre `\newcommand{\eqdef}{\overset{\text{def}}{=}}`), et
  rend visuellement différemment de `\coloneqq` du poly (171 emplois). Ce n'est
  pas la même convention pour « défini comme ».
- **`\norme{}`** — plus subtil : le commentaire de `td4.tex:7` affirme qu'elle
  est « fournie par le template ». Vrai, mais c'est l'alias déprécié de
  `ocots-compat.sty:100` pour `\norm{}` (177 emplois dans le poly, zéro
  `\norme`) — le rendu ne change pas, mais le TD pointe sans le savoir vers la
  voie de compatibilité plutôt que vers la macro actuelle.

## TD2 — Un énoncé se colle tel quel du TD au polycopié

C'est la promesse du template : `exercise`, `question`, `subquestion` sont les
mêmes environnements des deux côtés. **Ne pas réécrire un énoncé pour le
déplacer** — un exercice de TD promu en exercice de poly (ou l'inverse) se copie
sans retouche.

> **Sans prise dans le corpus.** `mesure-integration` ne partage aujourd'hui
> aucun exercice entre `td/` et `poly/` — pas de label commun, pas d'`\input`
> croisé. La règle reste un garde-fou correct pour le jour où un exercice migre
> d'un support à l'autre ; rien à mesurer pour l'instant.

## TD3 — Questions : les environnements, pas la numérotation manuelle

- `question` pour 1., 2., … · `subquestion` pour 2.1., 2.2., …
- **Jamais** de « 1) 2) 3) » saisis à la main, ni d'`enumerate` détourné pour
  numéroter des questions (règle [C6](communes.md#c6--listes-et-énumérations)).
- `\begin{exercise}` **n'accepte pas de titre libre**, seulement des clés :
  `label`, `points`, `nosolution`. Une citation de source
  (« d'après Sontag, ex. 1.4 ») se met en **texte d'intro** de l'exercice, on ne
  la perd pas.
- Un exercice sans texte d'intro : `\exercisenotext` en première ligne.

> **`exercise` est déjà la forme cible.** C'est le seul environnement du template
> qui prenne une liste clé-valeur et pose son label **tel quel**, sans préfixe
> ajouté — exactement ce que demande [C5](communes.md#c5--labels-et-renvois), et
> ce vers quoi le [chantier 1](template.md#chantier-1--signature-des-environnements-et-labels)
> veut amener tous les autres. Écrire `label=ex:matrices` puis citer
> `\ref{ex:matrices}` fonctionne donc **dès aujourd'hui**.

## TD4 — Progression

**Chaque exercice a un objectif identifiable** (une notion, une méthode). Un
exercice qui n'en a pas est un exercice à découper ou à retirer.

Le TD **s'ouvre sur quelque chose d'accessible** — une mise en jambe. Au-delà,
la progression n'est **pas un axe unique** facile → difficile : un TD qui
couvre plusieurs thèmes (tribu, application mesurable, mesure…) **équilibre
les thèmes**, plutôt que de grimper une seule pente. Alterner un exercice
simple sur un thème et un exercice plus dense sur le suivant est légitime,
et c'est souvent ce qui produit la meilleure séance — voir tous les thèmes
du chapitre plutôt qu'épuiser le plus facile avant d'attaquer le reste.

`mesure-integration/td1` le fait déjà, sans que la règle ait été écrite :
ex. 1–2 (applications mesurables), 3–4 (mesures), 5–6 (retour aux ensembles et
applications), 7 (retour à la tribu de Borel) — les thèmes alternent, la
difficulté ne grimpe pas en ligne droite.

À l'intérieur d'un exercice, en revanche, la chaîne reste stricte : les
questions **s'enchaînent**, une question prépare la suivante. Une question
isolée qui ne sert à rien de ce qui suit se signale — c'est un axe différent
de celui du TD entier, à l'échelle d'un seul exercice.

## TD5 — Citer le résultat du cours mobilisé, pas le redémontrer

**La base, c'est le `\ref`**, pas le nom : « D'après la
Proposition~\ref{prop:xxx} » suffit, même quand le résultat n'a pas de titre.
Si le résultat cité n'a pas encore de label, [P9](poly.md#p9--ne-pas-re-dériver-un-cas-particulier-dun-résultat-déjà-écrit)
et [P12](poly.md#p12--labels-ref-ables-uniquement-si-le-résultat-est-cité) le
disent déjà : on **ajoute** le label au poly, on ne réécrit rien.

**Le nom est un bonus, pas une obligation à fabriquer.** Quand le résultat a
un nom reconnu — « critère de Kalman », « théorème de convergence dominée »,
« théorème de Cauchy-Lipschitz » — l'utiliser rend le corrigé plus lisible que
« Théorème 4.4.6 ». Mais la **majorité** des résultats d'un poly n'ont pas de
nom (ce sont des étapes techniques, pas des résultats fondateurs) : ne pas en
inventer un. **Ne pas ajouter de titre à une boîte du poly dans le seul but de
pouvoir la nommer en TD** — un `\ref` sans nom reste conforme à la règle.

### Le corpus

```bash
grep -rc '\ref{' --include='*.tex' td/ | awk -F: '{s+=$2} END{print s}'
```

`mesure-integration` : **zéro** `\ref` dans tout `td/`. La moitié de la règle
est déjà suivie — `td2.tex:148` cite bien « le théorème de convergence
monotone » — mais sans renvoi, alors que le poly porte **deux** théorèmes
labellisés sous ce nom (`thm:Beppo-Levi-v0`, `thm:Beppo-Levi-pp`,
`theorems-limites.tex:173`). Rien à ajouter au poly ici : les labels existent
déjà, il manque le `\ref` côté TD.

### Le corpus

```bash
grep -rc '\ref{' --include='*.tex' td/ | awk -F: '{s+=$2} END{print s}'
```

`mesure-integration` : **zéro** `\ref` dans tout `td/`. La moitié de la règle
est déjà suivie — `td2.tex:148` cite bien « le théorème de convergence
monotone » — mais sans renvoi, alors que le poly porte **deux** théorèmes
labellisés sous ce nom (`thm:Beppo-Levi-v0`, `thm:Beppo-Levi-pp`,
`theorems-limites.tex:173`). Nommer sans citer laisse un lecteur qui voudrait
vérifier l'énoncé sans moyen d'y aller directement.

## TD6 — Corrigés : synthétiques, pour les intervenants

Le public d'un corrigé de TD, ce sont **les intervenants**, pas les étudiants.
Donc : la démarche, les étapes-clés, le résultat. **Pas un cours.**

- Une question = quelques lignes : méthode + résultat.
- Calcul long → étapes-clés seulement, résultat mis en évidence.
- Résultats numériques ou matriciels : **valeur finale explicite**.
- Notations **identiques à l'énoncé** (mêmes symboles, mêmes noms).
- Ton direct — on s'adresse à un collègue. Ne pas recopier l'énoncé.

### Le corpus

Trois corrections de `mesure-integration` dépassent 25 lignes
(`td1.tex:108`, `td3.tex:51` — 39 lignes —, `td4.tex:34`). La longueur seule
n'est pas la faute : TD6 tolère un calcul long réduit à ses étapes-clés. La
plus longue (`td3.tex:51`) ne l'est pas pour cette raison — elle **enseigne** :
deux apartés « Remarque » y proposent des méthodes alternatives (« On peut
aussi utiliser les résultats du chapitre 4… », « On pouvait aussi utiliser
directement le TCD… »), avec des justifications pédagogiques (« pour alléger
les notations »). C'est écrit pour un étudiant qui découvre, pas pour un
collègue qui enseigne — la distinction que pose la règle. Même exercice, même
théorème cité sans `\ref` qu'en [TD5](#td5--citer-le-résultat-du-cours-mobilisé-pas-le-redémontrer).

## TD7 — Où va le corrigé

- Le corrigé vit **dans le fichier de l'énoncé**, pas dans un `sol_tdN.tex`
  séparé. Le préambule passe de `solutions=none` à `solutions=inline` : une
  seule ligne change, et l'énoncé seul se retrouve avec `solutions=none`.
- Dans chaque `exercise`, après la dernière question : une ligne `\solution`,
  puis le corrigé. **`\solution` est une commande, pas un environnement**, et
  c'est un marqueur unique par exercice — tout ce qui suit est la correction.
- Hors boîte (remarque de séance, indication) : `\begin{correction}`.
- Exercice sans corrigé à fournir : `\begin{exercise}[nosolution]`.

### Le corpus

```bash
echo "exercises : $(grep -rc 'begin{exercise}' --include='*.tex' td/ | awk -F: '{s+=$2}END{print s}')"
echo "\solution : $(grep -rc '\\solution' --include='*.tex' td/ | awk -F: '{s+=$2}END{print s}')"
echo "correction : $(grep -rc 'begin{correction}' --include='*.tex' td/ | awk -F: '{s+=$2}END{print s}')"
```

| Cours | Exercices | `\solution` | `\begin{correction}` |
|---|---|---|---|
| `automatique` | 7 | **7** | 0 |
| `mesure-integration` | 22 | **0** | 30 |

`automatique` suit le mécanisme du template exactement : un `\solution` par
exercice. `mesure-integration` ne l'utilise **jamais** — chaque `exercise` se
ferme, puis une ou plusieurs `\begin{correction}` séparées suivent, le
mécanisme documenté pour l'usage inverse (« hors boîte, où l'énoncé n'est pas
encadré »). Conséquence vérifiée : `solutions=none/inline/end` ne pilote **pas**
ces corrigés — le préambule de `mesure-integration` a beau dire
`solutions=none`, les corrections s'affichent quand même.

Ces TD datent d'avant le template ocots (`% Olivier Cots, 13/10/2019` en
première ligne) : c'est un reliquat de migration, pas une exception voulue —
la classe `ocots-td` et les environnements `exercise`/`question` ont déjà
migré, seul le corrigé ne suit pas encore le mécanisme actuel.

## TD8 — Ne pas rouvrir `question` dans un `\solution`

Le compteur `question` **continue** celui de l'énoncé : le corrigé de la Q1
s'afficherait « 4 ». Structurer le corrigé par une **liste manuelle** qui
reprend les numéros de l'énoncé :

```latex
\begin{description}
  \item[1.] …
  \item[2.] …   \item[a.] …   \item[b.] …
\end{description}
```

Renvoi ponctuel à une question de l'énoncé : `\ref{…}` si elle porte un label,
sinon le numéro en clair. (`\newquestion` existe pour forcer le passage à la
question suivante quand le corrigé est rédigé à part.)

`automatique` suit ce patron à la lettre (`td1.tex:152`, `td2.tex:73` :
`\solution` puis `\begin{description}`, numéros repris à la main). Pas de
signal côté `mesure-integration` : sans `\solution` ([TD7](#td7--où-va-le-corrigé)),
le cas que cette règle vise ne peut pas se produire dans ce cours pour
l'instant.

## TD9 — Flottants dans un exercice

`\begin{figure}` ou `\begin{table}` **dans** un `exercise` ou une `question`
donne `! LaTeX Error: Float(s) lost.` (vérifié par compilation, y compris pour
une figure dans un `question` imbriqué dans un `exercise` — c'est la boîte
`exercise` qui décide, pas la présence du `question`). Sortir le flottant de la
boîte, ou le passer en non-flottant :

```latex
\begin{center}…\captionof{figure}{…}\end{center}
```

Puis vérifier que le `\label` / `\ref` qui le vise pointe toujours (règle
[C5](communes.md#c5--labels-et-renvois)). Même contrainte dans le polycopié,
[P13](poly.md#p13--figures), qui donne la liste complète des environnements
concernés — tout ce qui porte une décoration `tcolorbox`, `example` et `lemma`
exceptés.
