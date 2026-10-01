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

Sur `mesure-integration`, ce diff ne remonte plus de macro mathématique locale
au 1er octobre 2026 : `\shorttitle`, `\numero`, `\discipline` et `\promotion`
sont des métadonnées propres aux wrappers des TD ; `\iff` et `\searrow` sont
des commandes LaTeX standard. Les anciens écarts `\eqdef` / `\coloneqq` et
`\norme` / `\norm` ont été résorbés lors de l'harmonisation des TD.

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
grep -rc '\ref{' --include='contenu.tex' td/ | awk -F: '{s+=$2} END{print s}'
```

`mesure-integration` compte cinq `\ref` dans ses `tdN/contenu.tex`, tous vers
une question ou un exercice du même TD. Les résultats du poly restent cités par
leur nom et leur numéro en clair — par exemple le théorème de convergence
monotone dans `td/td2/contenu.tex` et `td/td3/contenu.tex` — plutôt que par un
renvoi. La règle n'est donc pas encore suivie pour ces citations du cours.

## TD6 — Corrigés : synthétiques, pour les intervenants

Le public d'un corrigé de TD, ce sont **les intervenants**, pas les étudiants.
Donc : la démarche, les étapes-clés, le résultat. **Pas un cours.**

- Une question = quelques lignes : méthode + résultat.
- Calcul long → étapes-clés seulement, résultat mis en évidence.
- Résultats numériques ou matriciels : **valeur finale explicite**.
- Notations **identiques à l'énoncé** (mêmes symboles, mêmes noms).
- Ton direct — on s'adresse à un collègue. Ne pas recopier l'énoncé.

### Le corpus

Les 39 corrections de `mesure-integration` respectent désormais ce calibrage :
aucune ne dépasse 25 lignes dans les `tdN/contenu.tex`; la plus longue occupe
23 lignes dans `td/td3/contenu.tex`. La longueur reste un signal, pas un
verdict : un calcul long réduit à ses étapes-clés peut être conforme à TD6.

## TD7 — Une source, deux versions

Chaque TD sépare le contenu commun de ses deux wrappers de compilation :

```text
tdN/contenu.tex
tdN/sujet.tex       — solutions=none
tdN/correction.tex  — solutions=inline
```

`contenu.tex` porte les énoncés et leurs corrigés. Les deux wrappers partagent
le même préambule, au titre et à l'option `solutions=` près, puis chargent ce
contenu par `\input{contenu}`. Il n'y a donc ni copie de l'énoncé, ni fichier
`sol_tdN.tex`, ni préambule à basculer à la main avant chaque compilation.

Dans un TD, un corrigé s'écrit avec l'environnement `correction` : il disparaît
en `solutions=none` et paraît en `solutions=inline`. La commande `\solution`,
qui sépare le haut et le bas d'un `exercise` et permet notamment le report avec
`solutions=end`, reste le mécanisme des exercices du polycopié.

### Le corpus

```bash
echo "exercises : $(grep -rc 'begin{exercise}' --include='contenu.tex' td/ | awk -F: '{s+=$2}END{print s}')"
echo "solution : $(grep -rc '\\solution' --include='contenu.tex' td/ | awk -F: '{s+=$2}END{print s}')"
echo "correction : $(grep -rc 'begin{correction}' --include='contenu.tex' td/ | awk -F: '{s+=$2}END{print s}')"
```

Au 1er octobre 2026 :

| Cours | Exercices | `\solution` | `\begin{correction}` |
|---|---:|---:|---:|
| `automatique` | 8 | 0 | 39 |
| `mesure-integration` | 21 | 0 | 39 |

Les deux cours de référence suivent donc la même architecture et la même
commande de filtrage.

## TD8 — Un corrigé au fil des questions

Chaque `correction` suit **immédiatement** la `question` ou `subquestion`
qu'elle corrige, à l'intérieur de l'`exercise`. Elle est un environnement frère
de la question, pas un environnement imbriqué dedans :

```latex
\begin{question}
    Montrer que la suite converge.
\end{question}
\begin{correction}
    Appliquer le théorème de convergence dominée.
\end{correction}
```

Si une même démarche corrige plusieurs questions consécutives, la correction
suit la dernière question concernée et l'annonce explicitement. Un exercice
sans `question` place sa correction directement après son énoncé, toujours
avant `\end{exercise}`.

Cette proximité conserve l'association entre l'énoncé et sa correction sans
rouvrir `question`, sans recopier les numéros dans une liste `description` et
sans employer `\newquestion`. Un renvoi utile à une autre question se fait
normalement par `\label` / `\ref`.

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
