<!-- LTeX: language=fr-FR -->

# Règles de rédaction — sujets d'examen

Classe `ocots-exam`. Prérequis : [`communes.md`](communes.md) et
[`td.md`](td.md) (un sujet d'examen partage l'essentiel de la mécanique d'un TD :
`exercise`, `question`, `subquestion`, `\solution`).

> **État.** `EX1`–`EX7` sont éprouvées sur le corpus (14 sujets de
> `mesure-integration`, 2019–2025). `EX5` reste sans prise mécanique — un
> jugement de relecture, comme TD2/TD4 avant révision.

Identifiants : `EX1`, `EX2`, … (voir [`README.md`](README.md#citer-une-règle--épingler-la-version)).

---

## EX1 — L'énoncé est autonome

Un sujet se lit et se traite **sans le polycopié sous les yeux**, sauf si les
documents sont explicitement autorisés (et alors le sujet le dit).

Conséquence pratique : un résultat du cours mobilisé par le sujet est soit
**rappelé dans l'énoncé**, soit **nommé sans ambiguïté** (« le théorème de
convergence dominée »). **Pas de `\ref{…}` vers le polycopié** dans un sujet :
c'est un autre document, le renvoi ne résout donc pas — et
[C5](communes.md#c5--labels-et-renvois) exige zéro référence non résolue.

C'est la différence avec un TD, qui lui **cite** le cours
([TD5](td.md#td5--citer-le-résultat-du-cours-mobilisé-pas-le-redémontrer)) : en TD le polycopié est
à portée de main, à l'examen non.

### Le corpus

Clean sur les 14 sujets : chaque `\ref{…}` pointe vers une partie ou un
exercice **du sujet lui-même** (`partie:martial`, `exo:olivier1`…), jamais
vers le poly.

## EX2 — Conditions annoncées en tête

Les métadonnées de la classe portent l'identité du document (`\title`,
`\numero`, `\date`, `\discipline`, `\promotion`). En tête du sujet, dans un
`instructions` :

- **durée** ;
- **documents et calculatrice** autorisés ou non ;
- **barème** (indicatif ou ferme) ;
- indépendance des parties, s'il y en a (règle EX4).

### Le corpus

Sur les 14 sujets de `mesure-integration` (2019–2025), la **durée manque dans
5** — les plus anciens (2019-2020 ×2, 2020-2021 ×3). Devenue systématique
depuis 2021, mais rien ne la rend obligatoire : un sujet qui l'omet compile
sans se plaindre. Documents et calculatrice, eux, sont présents dans les
**14** sujets.

> **En attendant la révision du template.** `instructions` est aujourd'hui un
> `itemize` écrit à la main, sans structure ni ordre fixe. Le
> [chantier 4](template.md#chantier-4--champs-dexamen-et-barème-calculé)
> propose des champs dédiés (`\duree`, `\documents`, `\calculatrice`), sur le
> modèle de `\title`/`\author` — un champ non renseigné manquerait
> visiblement plutôt que silencieusement.

## EX3 — Barème par les clés, pas à la main

`\begin{exercise}[points=4]` affiche `(4 points)` après le numéro. Ne pas écrire
le barème dans le texte de l'énoncé : il se désynchronise à la première
retouche.

Le total des `points` doit tomber juste — **sur quoi que ce soit**, 20 n'a rien
d'obligatoire. C'est une vérification de relecture à part entière, en attendant
que le [chantier 4](template.md#chantier-4--champs-dexamen-et-barème-calculé)
le rende automatique.

### Le corpus

`points=` : **24 emplois** au total sur les 14 sujets, mais adopté puis
abandonné d'une session à l'autre, par le même auteur — 100 % en 2020-2021,
0 % en 2021-2022, 100 % puis 0 % en 2022-2023 et 2023-2024. Quand `points=`
est absent, le barème reste écrit **en prose libre** dans `instructions`
(« barème prévisionnel : 8 points ») — sans lien avec les exercices, et sans
rien qui empêche les deux de diverger.

## EX4 — Parties indépendantes, et le dire

Une question ratée ne doit pas bloquer tout le sujet. Structurer en `docpart`
(Partie 1, 2, …) et **annoncer explicitement** l'indépendance quand elle existe
(« Les trois parties sont indépendantes. »).

À l'intérieur d'une partie, une question qui dépend d'un résultat non obtenu se
formule pour rester traitable (« On admet désormais que… »).

### Le corpus

`docpart` : 13/14 sujets structurés, seul `exam2019_session2.tex` (le plus
ancien) reste plat. Indépendance annoncée dans 10/14 ; les 4 absences
(`exam2019.tex`, `exam2019_session2.tex`, `exam2021_1bis.tex`,
`exam2021_2.tex`) suivent le même schéma de maturation que la durée
([EX2](#ex2--conditions-annoncées-en-tête)) — concentrées dans les débuts.

## EX5 — Calibrage

Le sujet est calibré pour la durée annoncée. Les repères pratiques du cours
(nombre de questions, longueur des calculs, proportion cours / application) se
consignent dans le dépôt de cours — ils dépendent de la promotion.

Un exercice repris d'un TD ou d'une annale se **signale en relecture** : c'est
une décision pédagogique, pas un détail.

> **Sans prise mécanique.** Ni label partagé ni contenu dupliqué détecté entre
> `exams/` et `td/` sur `mesure-integration` — mais une reprise ne se voit pas
> forcément à un label commun. Reste un jugement de relecture, comme
> [TD2](td.md#td2--un-énoncé-se-colle-tel-quel-du-td-au-polycopié).

## EX6 — Corrigé et barème détaillé

Même mécanique qu'en TD (règles [TD6](td.md#td6--corrigés--synthétiques-pour-les-intervenants)
à [TD8](td.md#td8--ne-pas-rouvrir-question-dans-un-solution)) : `\solution` dans
l'`exercise`, `solutions=inline` pour la version corrigée, `solutions=none` pour
le sujet distribué.

Spécifique à l'examen : le corrigé porte la **répartition des points à
l'intérieur de l'exercice** — c'est ce qui rend la correction reproductible
entre correcteurs.

### Le corpus

**Zéro `\solution`, zéro `\begin{correction}`** dans les 59 exercices des 14
sujets — aucun corrigé écrit, pour aucune année. Différent du cas
[TD7](td.md#td7--où-va-le-corrigé) : là un corrigé existait mais par le
mauvais mécanisme ; ici il n'existe simplement pas encore dans le dépôt.

## EX7 — Notations identiques au cours

Règle [C2](communes.md#c2--cohérence-terminologique-et-notationnelle), appliquée
avec sévérité : un symbole qui change de sens entre le cours et l'examen est une
faute de sujet, pas une variante.

### Le corpus

Le même macro-diff qu'en [TD1](td.md#td1--le-polycopié-fait-référence),
appliqué à `exams/` vs `poly/` :

- **`\defeqq`** — redéfinie localement dans **quatre** sujets distincts (2021,
  2022-s2, 2023-s1, 2023-s2), chacun avec son propre
  `\newcommand{\defeqq}{\overset{\mathrm{def}}{=}}`. Le jumeau exact du `\eqdef`
  trouvé en TD1 — même geste, nom différent, même divergence avec `\coloneqq`
  du poly (171 emplois).
- **`\Acal`** — tribu générique dans `exam2019.tex` et `exam2021_2.tex`
  (« La famille $\Acal = \ldots$ »), quand le poly nomme sa tribu générique
  `\AT` dans ses 280 emplois. Deux noms pour le même rôle structurel.

Deux occurrences indépendantes du même geste (réinventer « défini comme »)
suggèrent un manque réel de visibilité de `\coloneqq`, pas un accident isolé.
