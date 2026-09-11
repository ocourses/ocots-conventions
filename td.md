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

Les exercices vont du plus simple au plus complexe, et **chaque exercice a un
objectif identifiable** (une notion, une méthode). Un exercice qui n'en a pas
est un exercice à découper ou à retirer.

À l'intérieur d'un exercice, les questions **s'enchaînent** : une question
prépare la suivante. Une question isolée qui ne sert à rien de ce qui suit se
signale.

## TD5 — Nommer le résultat du cours mobilisé

Le corrigé **cite** le théorème utilisé (« critère de Kalman », « théorème de
convergence dominée », « théorème de Cauchy-Lipschitz ») plutôt que de le
redémontrer. Renvoi `\ref{…}` vers le chapitre du poly s'il porte un label.

## TD6 — Corrigés : synthétiques, pour les intervenants

Le public d'un corrigé de TD, ce sont **les intervenants**, pas les étudiants.
Donc : la démarche, les étapes-clés, le résultat. **Pas un cours.**

- Une question = quelques lignes : méthode + résultat.
- Calcul long → étapes-clés seulement, résultat mis en évidence.
- Résultats numériques ou matriciels : **valeur finale explicite**.
- Notations **identiques à l'énoncé** (mêmes symboles, mêmes noms).
- Ton direct — on s'adresse à un collègue. Ne pas recopier l'énoncé.

## TD7 — Où va le corrigé

- Le corrigé vit **dans le fichier de l'énoncé**, pas dans un `sol_tdN.tex`
  séparé. Le préambule passe de `solutions=none` à `solutions=inline` : une
  seule ligne change, et l'énoncé seul se retrouve avec `solutions=none`.
- Dans chaque `exercise`, après la dernière question : une ligne `\solution`,
  puis le corrigé. **`\solution` est une commande, pas un environnement**, et
  c'est un marqueur unique par exercice — tout ce qui suit est la correction.
- Hors boîte (remarque de séance, indication) : `\begin{correction}`.
- Exercice sans corrigé à fournir : `\begin{exercise}[nosolution]`.

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
