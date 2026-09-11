<!-- LTeX: language=fr-FR -->

# Règles de rédaction — polycopié

Dégagées pendant la relecture ligne à ligne de
`calcul-differentiel-edo-enseignants`, puis éprouvées sur
`automatique-enseignants`.

Prérequis : [`communes.md`](communes.md) (langue, notations, macros,
typographie, labels).

> **Citation antérieure.** Les journaux de relecture de ces deux cours citent
> ces règles par un nombre nu — « règle 5 », « règles 3, 4, 13 ». Ce sont les
> mêmes : **P*n* était la règle *n*** (voir
> [`CHANGELOG.md`](CHANGELOG.md)). Depuis, une relecture épingle la version des
> conventions qu'elle applique, ce qui permet de réordonner les règles.

> **Le fil conducteur.** On n'enchaîne jamais des boîtes (`definition`,
> `theorem`, `proposition`, `corollary`, `remark`, `example`) sans texte autour.
> Chaque boîte est amenée par une phrase qui dit *pourquoi elle arrive*, et
> suivie d'une phrase qui *exploite* ce qu'elle apporte. Le lecteur ne doit
> jamais tomber sur un énoncé sans savoir ce qu'on cherche à en faire.

---

## P1 — Placement des hypothèses

> **Le critère est unique : un résultat cité de loin réénonce ses hypothèses.**

Un lecteur arrive sur une boîte de deux façons — en lisant dans l'ordre, ou par
un renvoi. Le second n'a pas le texte qui précède sous les yeux : **si l'objet
est cité de loin, il doit se suffire à lui-même.**

Ce n'est pas le type d'environnement qui décide, c'est cet usage. Et il est
observable : *cité de loin* = **porte un label qui est `\ref`-encé ailleurs**
(règle [C5](communes.md#c5--labels-et-renvois)).

```bash
grep -rn 'ref{thm:cauchy}' --include='*.tex' . | wc -l
```

### Ce que le critère donne, par environnement

| | Hypothèses | Pourquoi |
|---|---|---|
| **théorème** | **dedans**, toujours | un théorème est fait pour être cité |
| **lemme** | **dedans** | il est sorti du texte précisément pour être réutilisé ailleurs |
| **corollaire** | selon qu'il est cité ou non | s'il n'est qu'une lecture immédiate de ce qui précède, il s'appuie sur le décor |
| **proposition** | selon le contexte | prolonge le texte (mêmes objets) → dehors ; ouvre un cadre nouveau → dedans |
| **définition** | **dehors**, dans la phrase qui précède | la boîte se concentre sur la seule condition définissante |

**Exception** : après une longue digression, une définition qu'un lecteur voudra
lire isolément garde ses hypothèses dedans, comme un théorème.

### Le corpus le confirme déjà

Dans `calcul-differentiel-edo-enseignants`, les trois corollaires du chapitre sur
le flot se répartissent exactement ainsi — sans que la règle ait été écrite :

| Corollaire | Étiqueté | Hypothèses |
|---|---|---|
| « Le flot $\vphi$ est défini sur un ouvert $\D$ » | non | aucune — s'appuie sur le décor |
| `resolvante` | oui | partielles (« Soit $\xsol_0 \in \Omega$ et $t \in I(\xsol_0)$ ») |
| `globale` | oui | complètes (« Soit $f \colon \Ical\times\R^n\to\R^n$ continue… ») |

Et les quatre lemmes y sont **tous** étiquetés, tous avec leurs hypothèses
dedans, tous sortis au niveau du texte — **jamais à l'intérieur d'une preuve**.
Un lemme qu'on hisse hors d'une preuve, c'est un lemme qu'on veut pouvoir citer :
il suit donc la règle du théorème. Un argument qui ne sert qu'une fois, à un seul
endroit, n'a pas besoin d'être un lemme — il reste une étape de la preuve,
séparée par `\newstep`.

### La redondance avec le décor est voulue

Le décor de section ([P7](#p7--ouverture-de-chapitre-et-de-section)) **ne
dispense jamais** un résultat cité de le réénoncer. Les deux servent deux
lecteurs différents : le décor sert celui qui lit dans l'ordre, P1 sert celui qui
arrive par un `\ref`. Répéter les hypothèses d'un théorème déjà posées trois
lignes plus haut n'est pas une faute, c'est la règle.

## P2 — Pas de blocs isolés ou enchaînés sans texte

- Toujours **au moins une phrase de liaison** entre deux boîtes (définition →
  proposition, proposition → corollaire…). **Jamais deux ou trois boîtes collées
  sans texte entre elles.**
- Une remarque qui n'ajoute qu'un alias de notation ou une précision mineure
  (« on note aussi… », « ne pas confondre… ») se **fond** dans la définition ou
  le résultat qui précède, plutôt que de rester une boîte à part — elle échoue
  au test de [P5](#p5--ce-quest-une-remark).

### Deux exceptions

**Une série d'exercices.** `exercise` → `exercise` est légitime : une série en
fin de chapitre est une **liste**, pas une narration, et n'a pas à être
commentée entre chaque item.

**L'entrée dans une remarque.** Une `remark` peut suivre directement n'importe
quelle boîte, y compris une autre `remark` : elle se rattache à ce qui précède,
elle n'a pas à en être détachée par une phrase
([P5](#p5--ce-quest-une-remark)).

L'exception ne vaut que pour ce qui *entre* dans la remarque. **Ce qui en sort
reste soumis à la règle** : une `definition` collée après une `remark` se
retrouve sans amorce dès qu'on saute la remarque — et une remarque, par
définition, se saute.

Il n'y a **pas** d'exception pour les autres :

- `example` → `example` : chaque exemple s'amorce **individuellement**, par une
  phrase qui dit ce qu'il illustre ([P3](#p3--une-phrase-damorce-motivée-avant-chaque-boîte)) ;
- `definition` → `definition` : une phrase entre chaque, même pour une série de
  notions liées (contrôle / sortie / consigne).

### La règle se mesure

Un `\end{boîte}` suivi d'un `\begin{boîte}` sans rien entre les deux est
détectable exactement :

```bash
./conventions/bin/verifier P2 poly/
```

Relevé sur les quatre polycopiés, à la première passe :

| Polycopié | Enchaînements | dont entrées en `remark`, tolérées | Reste | État |
|---|---|---|---|---|
| `automatique` | 8 | 2 | **6** | passe P2 complète faite |
| `calcul-differentiel-edo` | 36 | 10 | 26 | partiellement relu |
| `controle_optimal` | 67 | 26 | 41 | non relu |
| `mesure-integration` | 106 | 40 | **66** | non relu |

Le seul polycopié ayant reçu la passe est à 6, celui qui n'a rien reçu est à 66.
Le compte suit le travail : il fait une bonne **baseline** au sens de
[`methode.md`](methode.md), et une salve peut s'annoncer par la baisse qu'elle
produit.

La colonne du milieu est l'exception de [P5](#p5--ce-quest-une-remark), ajoutée
après la première rédaction de cette règle : elle absorbe entre un quart et
**38 %** des enchaînements relevés. Une baseline n'est donc comparable qu'à
version de conventions égale — d'où
[l'épinglage](README.md#citer-une-règle--épingler-la-version).

## P3 — Une phrase d'amorce *motivée* avant chaque boîte

La phrase qui précède une boîte ne fait pas que la jonction. **Elle répond à au
moins une de ces questions :**

| | Exemple du corpus |
|---|---|
| **à quoi sert le résultat ?** | « La propriété suivante est fondamentale, elle permet de calculer de nombreuses dérivées. » |
| **que fournit-il exactement ?** | « Cette première forme ne requiert que la différentiabilité $k$ fois au point $x$ lui-même et fournit un reste seulement négligeable : c'est la version adaptée à l'étude locale. » · « On obtient ainsi la condition nécessaire suivante, d'optimalité locale faible à l'ordre deux. » |
| **d'où vient-il ?** | « …qui n'est qu'un cas particulier de \cite[Lemme~2.6.2]{Sontag:1998} » · « …qui n'est qu'une simple conséquence du lemme de Morse » |
| **à quoi se rattache-t-il ?** | « …analogue au Théorème~\ref{thm:appliOuverteNL}, mais à l'ordre 2 » · « comme corollaire du Théorème~\ref{thm:regulariteFlot} » |
| **que va-t-on en faire ?** | « Donnons maintenant une caractérisation géométrique du premier instant conjugué. » |

### Ce n'est pas l'ouverture qui est fautive, c'est l'arrêt

« Nous avons le résultat suivant. » n'est pas interdit **comme ouverture** — il
l'est comme **phrase entière**. Comparer :

> ❌ « Nous avons alors le théorème suivant. »
> ✅ « Nous avons alors le théorème suivant **qui n'est qu'un cas particulier
> de~\cite[Lemme~2.6.2]{Sontag:1998}, nous assurant l'existence et l'unicité
> d'une solution maximale.** »

Le test : **retirer l'annonce, reste-t-il une information ?**

**Ne jamais laisser une phrase se jeter dans une boîte** (« On peut donc donner
la » suivi de `\begin{definition}`) : l'amorce est une phrase complète.

### Les exercices aussi

Un **exercice isolé** au fil du texte s'amorce comme les autres boîtes : on dit
ce qu'il fait travailler. L'exception de [P2](#p2--pas-de-blocs-isolés-ou-enchaînés-sans-texte)
ne porte que sur les **séries** d'exercices en fin de chapitre, où la liste se
présente une fois pour toutes.

### Deux signaux automatisables

```bash
./conventions/bin/verifier P3 poly/
```

Ce sont des **signaux, pas des verdicts** — une amorce motivée ne se juge pas
mécaniquement. L'outil repère seulement deux formes sûres :

- la phrase d'annonce qui **s'arrête** à l'annonce ;
- la phrase qui **ne se termine pas** avant `\begin{…}`.

Là encore, le polycopié qui a reçu la passe (`automatique`) est à **0**, contre
13 et 14 pour ceux qui ne l'ont pas reçue.

## P4 — Un résultat qui n'est pas exploité n'a pas été posé

> **Après un résultat, quelque chose l'exploite. Ce n'est pas forcément une
> phrase.**

L'exploitation prend plusieurs formes, et c'est la nature du résultat qui
décide :

| Forme | Quand |
|---|---|
| une **phrase** qui interprète ce qu'on vient d'obtenir | le résultat parle de lui-même, il suffit de le situer |
| un **exemple** qui l'instancie | l'énoncé est abstrait |
| une **discussion des hypothèses** — peut-on les affaiblir ? | les hypothèses sont nombreuses ou surprenantes |
| un **contre-exemple** montrant qu'une hypothèse ne se retire pas | une hypothèse a l'air gratuite |
| le **résultat suivant**, s'il en découle | corollaire, spécialisation |

**Aucune de ces formes ne tient dans une `remark`.** Une remarque est
optionnelle par définition ([P5](#p5--ce-quest-une-remark)) : ce qu'on peut
sauter n'exploite rien. Une discussion des hypothèses écrite en remarque laisse
donc le résultat non exploité — c'est le motif le plus fréquent du corpus.

### La reprise vient après l'unité, pas après la boîte

`\end{theorem}` suivi de `\begin{proof}` est **le successeur le plus fréquent
d'une boîte** dans le corpus — 147 occurrences — et il est correct. La reprise
se place après l'**énoncé et sa preuve**, jamais entre les deux.

### Entre deux boîtes, une seule phrase porte les deux rôles

Une phrase placée entre un théorème et son corollaire est **à la fois** la
reprise du premier et l'amorce du second : P3 et P4 sont deux critères sur la
**même** phrase, pas deux phrases à écrire.

Elle dit donc *ce que la spécialisation apporte* — « En prenant $E = \R^n$, on
obtient la forme habituelle du théorème des accroissements finis », « Le cas
linéaire homogène en découle immédiatement » — et jamais « On en déduit le
corollaire suivant. », qui annonce sans rien dire (détecté par
[P3](#p3--une-phrase-damorce-motivée-avant-chaque-boîte)).

### Un signal, pas une infraction

**73 sections du corpus se terminent sur une boîte**, sans rien après. C'est
détectable, mais ce n'est pas automatiquement une faute : l'ouverture de la
section suivante peut faire le travail.

La question à se poser est celle de l'exploitation, pas celle de la ponctuation
de fin de section : *ce théorème, en a-t-on montré un cas concret ? a-t-on dit
si ses hypothèses peuvent être relâchées ?* Si la réponse est non nulle part,
il manque quelque chose — que ce soit avant ou après le titre suivant.

## P5 — Ce qu'est une `remark`

> **Une remarque est mise en avant, optionnelle, et souvent tournée vers
> l'extérieur.**

Les trois propriétés comptent, et c'est la deuxième qui tranche :

- **mise en avant** — si le contenu se lisait aussi bien au fil du texte, il y
  reste ; la boîte est ce qui l'en sort ;
- **optionnelle** — *un lecteur qui saute toutes les remarques ne perd rien de
  ce qu'il lui faut pour suivre le cours et réussir l'examen* ;
- **tournée vers l'extérieur** — elle relie souvent ce qui précède à autre chose
  que le cadre courant : un autre résultat, un autre cours, un contexte
  historique, une ouverture.

### Le test

**Retirer la boîte. Le lecteur a-t-il perdu quelque chose dont il a besoin ?**

- **Non** → c'est une remarque. Elle reste, où qu'elle soit placée.
- **Oui** → ce n'est pas une remarque, quel que soit le ton. Le contenu est
  nécessaire, donc il rejoint le fil.

**Il n'y a pas de critère de position.** Une remarque qui commente le résultat
juste au-dessus est parfaitement légitime — c'est même l'un de ses emplois les
plus utiles, dès lors qu'elle le met en relation avec autre chose. Ce qui
disqualifie une remarque, ce n'est pas de parler de la boîte précédente, c'est
d'être **indispensable**.

### L'optionalité se vérifie dans le corpus

Sur les quatre polycopiés : **176 remarques, pas un seul label.** Personne ne
cite jamais une remarque — ni le polycopié, ni les TD, ni les examens. Ce qu'on
met en remarque est de fait retiré de ce qui se cite.

C'est ce qui rend le test tranchant : **un énoncé dont on aura besoin plus loin
ne peut pas rester dans une remarque**, il y serait incitable.

### Quand ce n'en est pas une

| Contenu | Perdu si on le saute ? | Destination |
|---|---|---|
| un **énoncé mathématique** utilisé ou cité plus loin | oui | `proposition` / `corollary` — et il devient citable |
| **ce qu'il faut retenir** du résultat précédent | oui | texte courant : c'est l'exploitation ([P4](#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé)) |
| une **convention de notation ou de vocabulaire** employée ensuite | oui | texte courant, là où les objets sont introduits |
| une **instanciation** sur un cas concret | — | `example` ([P6](#p6--remark-vs-example)) |
| un **rappel supposé acquis**, un lien URL | non, mais il ne faut pas s'arrêter | **note de bas de page** (voir plus bas) |
| une **mise en garde**, un cas limite, un contre-exemple ponctuel | non | `remark` |
| un **lien** avec un autre cadre, un autre cours, un résultat ultérieur | non | `remark` |
| un **point historique**, un renvoi « pour aller plus loin » | non | `remark` |

### Une remarque ne peut pas être l'exploitation d'un résultat

Conséquence directe de l'optionalité. Si la seule chose qui suit un théorème est
une remarque, le théorème n'a pas été exploité au sens de
[P4](#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé) : la remarque, par
définition, on peut la sauter.

Le cas typique du corpus est `produits.tex:131` — « On retient que pour des
fonctions mesurables positives, on peut intervertir l'ordre des intégrations »,
en remarque juste après Fubini-Tonelli. C'est *le* point à retenir du théorème :
il va au fil du texte. Ce qui peut rester en remarque, c'est ce qui **ouvre
ailleurs**.

Même raison pour l'amorce : une remarque ne fait pas la liaison vers la boîte
suivante. Un `remark` suivi d'une `definition` collée laisse la définition sans
amorce dès qu'on saute la remarque — c'est
[P3](#p3--une-phrase-damorce-motivée-avant-chaque-boîte) qui reprend la main.

### Enchaîner des remarques n'est pas une faute

Plusieurs apartés distincts peuvent se rattacher au même résultat, et rien
n'oblige à les relier par une phrase. Une remarque suit directement une boîte, y
compris une autre remarque —
[P2](#p2--pas-de-blocs-isolés-ou-enchaînés-sans-texte) ne s'applique pas à ce
qui *entre* dans une remarque.

**Trois est la limite ; quatre est un signal.** Pas un signal de ponctuation :
un signal de contenu. Au-delà de trois, ou bien plusieurs de ces boîtes ont
échoué au test — le contenu n'était pas optionnel —, ou bien le sujet mérite un
traitement à lui : un paragraphe de discussion, une sous-section.

Le corpus donne au seuil sa calibration : sur quatre polycopiés, la longueur 4
n'apparaît que **deux fois**, toutes deux dans `controle_optimal`.

| Chaîne | Ce qu'on y trouve |
|---|---|
| `shooting.tex:133` | la fonction de tir, ce que signifie son zéro, le lien avec le hamiltonien vrai — le cœur de la méthode, mis en marge |
| `calcul-des-variations.tex:2104` | trois affaiblissements successifs des hypothèses de la proposition précédente — une discussion qui appelle un paragraphe |

Aucune des deux n'est optionnelle. Le seuil ne détecte pas un excès de boîtes,
il détecte **un contenu qui a quitté le fil**.

### `remark` ou note de bas de page

Les deux ne servent pas le même lecteur.

| | Note de bas de page | `remark` |
|---|---|---|
| Sert | celui qui a **besoin d'un rappel** | celui qui **veut aller plus loin** |
| Contenu | une définition venue d'un autre cours et supposée acquise, un lien URL, une référence précise | un lien avec un autre cadre, un point historique, une mise en garde, une ouverture |
| Le lecteur qui n'en a pas besoin | **ne s'arrête pas** | est **invité** à s'arrêter |

Une note **dépanne sans interrompre** ; une remarque **propose**. D'où les deux
conséquences :

- un **point historique** ou une **mise en relation** ne vont pas en note de bas
  de page, même courts — ce sont des choses qu'on offre, donc qu'on montre ;
- un **rappel supposé acquis** ne va pas en remarque — la boîte lui donnerait un
  poids qu'il n'a pas.

Le cas reste rare : la note de bas de page est un outil d'appoint, pas une
destination fréquente pour ce qui est aujourd'hui en remarque.

### Cette règle ne s'outille pas

« Le lecteur en a-t-il besoin ? » relève du jugement de l'auteur, et aucune
formulation ne le remplace. Le seul indicateur automatisable est la **longueur
des chaînes**, et il ne dit pas où est la faute — seulement où regarder.

```bash
./conventions/bin/verifier P5 poly/
```

## P6 — `remark` vs `example`

Une boîte qui **instancie** un résultat général sur un cas concret (objets
précis substitués) est un `example`, **pas** une `remark`. Un contre-exemple
aussi : `example[Contre-exemple]`.

L'exemple est amené par une phrase qui dit **ce qu'il illustre**, **placée avant
la boîte** (« … cette inclusion est toujours stricte, `\cf` l'exemple
suivant. »). C'est ce qui le sépare de la remarque du point de vue de
[P2](#p2--pas-de-blocs-isolés-ou-enchaînés-sans-texte) : un `example` s'amorce,
une `remark` se rattache.

Les deux ne jouent d'ailleurs pas le même rôle. Un exemple **sert la
compréhension du résultat** : celui qui le saute peut ne pas comprendre
l'énoncé. Il échoue donc au test de [P5](#p5--ce-quest-une-remark) — ce n'est
pas un aparté, c'est une pièce du fil, et c'est aussi l'une des formes
d'exploitation que [P4](#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé)
accepte.

Un contre-exemple qui montre qu'une hypothèse ne se retire pas est dans le même
cas. Un contre-exemple d'ouverture — « ailleurs, ça se passe autrement » — est
une remarque.

## P7 — Ouverture de chapitre et de section

### Le chapitre : une ossature fixe

```latex
\chapter{Théorie de la mesure}%
\label{chap:theorie-mesure}%
\minitoc%

<introduction du chapitre>

\clearpage
\section{Espaces mesurables}
```

`minitoc` est chargé par le support livre (`ocots-carrier-book.sty:27`, avec
`minitocdepth=2`) — rien à faire d'autre que l'appeler.

L'**introduction de chapitre** dit où l'on va et pourquoi : l'objet qu'on
cherche à construire, les notions qu'il faudra pour y arriver, dans quel ordre.
Elle s'adresse à un lecteur qui n'a encore rien lu du chapitre, se termine
souvent par ce que le chapitre établit (« C'est ce que nous faisons dans ce
chapitre. »), et peut renvoyer aux autres chapitres. Deux modèles :

- `mesure-integration-enseignants`, chapitre « Théorie de la mesure » : part de
  l'intégrale à définir, puis introduit tribu, ensemble mesurable, mesure et
  application mesurable, chacune motivée par la précédente ;
- `controle_optimal`, chapitre « Systèmes dynamiques contrôlés » : enchaîne les
  notions centrales du chapitre (accessible, contrôlabilité, application
  entrée/sortie, contrôle singulier) en disant à chaque fois pourquoi elle
  compte.

Elle est composée **en retrait**, ce qui la distingue du corps du chapitre.

Puis **un saut de page** : l'introduction n'a pas à partager sa page avec la
première section.

> **En attendant la révision du template.** Le retrait s'obtient aujourd'hui en
> détournant `quote` ou `quotation`, et le corpus est incohérent —
> `controle_optimal` emploie les deux, dans le même polycopié. Un environnement
> dédié est demandé au [chantier 3](template.md#chantier-3--correctifs-divers).

### La section : trois outils, selon le cadre

Il n'y a **pas** d'ouverture obligatoire pour une section. Ce qui décide, c'est
le **poids du cadre** et le **nombre de résultats qui le partagent** :

| Le cadre est… | partagé par… | Outil |
|---|---|---|
| léger (un ou deux objets) | un seul résultat | **rien** — les objets vont dans la boîte |
| léger | plusieurs résultats de la section | **décor** en tête de section |
| lourd (plusieurs conditions) | plusieurs résultats, cités | bloc **`assumption`**, référencé |

**Le décor** (la « cast list ») repose les objets courants avant la première
boîte :

> Soient $(E,\norm{\cdot}_E)$ et $(F,\norm{\cdot}_F)$ deux espaces vectoriels
> normés, $U$ un ouvert de $E$, une application $f \colon U \to F$ et un point
> $x \in U$.

ou, s'il est déjà en place plus haut : « Rappelons que $f \colon U \subset E \to
F$ désigne une application et $U$ un ouvert de $E$. » Il est surtout utile quand
une section a **plusieurs sous-sections** qui travaillent sur les mêmes objets.

**Le bloc `assumption`** (environnement du template, étiqueté `H1`, `H2`…) sert
quand le cadre est trop lourd pour être répété. C'est l'outil le moins fréquent,
réservé aux cadres vraiment encombrants — 8 emplois dans tout le poly de
contrôle optimal, contre 24 théorèmes.

Il y porte le cadre complet d'un problème (le système contrôlé, les applications
lisses, les ouverts, les contraintes), amené par une phrase — « Rappelons les
hypothèses sur les données du problème. » — et les énoncés s'y réfèrent ensuite :

```latex
\begin{assumption}%
  \label{hyp:ocp}
  Soit un système contrôlé non autonome $\dot{x}(t) = f(t,x(t),u(t))$ où $f$ est
  une application lisse de $\Ical \times \Omega \times \Ucal$ dans $\R^n$…
\end{assumption}

… sous les Hypothèses~\eqref{hyp:ocp}, …
```

Le renvoi se fait avec **`\eqref`**, pas `\ref` (règle
[C5](communes.md#c5--labels-et-renvois)).

> **Forme actuelle du label.** `assumption` est bâti sur `\newtheorem`, comme
> `lemma`, `example` et `remark` : le label se pose par `\label{…}` dans le
> corps, pas par une clé. La forme `[label=hyp:ocp]` est la **cible** du
> [chantier 1](template.md#chantier-1--signature-des-environnements-et-labels),
> pas la syntaxe d'aujourd'hui.

### La phrase d'introduction est un quatrième outil, indépendant

Elle ne parle pas des objets mais du **rôle** de la section : ce qu'on va y
faire, et quel est le résultat qui compte. Elle se combine avec n'importe lequel
des trois cas ci-dessus, ou s'emploie seule.

Elle n'est **pas obligatoire** — mais une section de plus de deux ou trois
boîtes en gagne presque toujours une.

### Cohérence avec la page de notations

La **liste des objets courants est propre à chaque cours** : elle se consigne
dans `reports/<passe>/06-regles-style-relecture.md` du dépôt de cours, en
cohérence avec la page de notations (règle
[P14](#p14--structure-du-polycopié)). **Ne pas redéfinir dans le corps ce qui y
est déjà fixé** — mais voir [P1](#p1--placement-des-hypothèses) : un résultat
cité de loin réénonce quand même ses hypothèses, et cette redondance-là est
voulue.

## P8 — Ne pas répéter une hypothèse

Si une hypothèse (« $E$ Hilbert », « $f$ de classe $\xCn{1}$ ») sert à plusieurs
conséquences dans un même exemple ou une même preuve, la poser **une seule
fois**, à l'endroit qui couvre toutes ses conséquences — jamais répétée à chaque
usage.

## P9 — Ne pas re-dériver un cas particulier d'un résultat déjà écrit

Si un second énoncé n'est que la **restriction** ou un **cas particulier** d'un
résultat déjà donné plus haut, y **renvoyer** plutôt que de le réafficher ou de
le redémontrer en entier. Cela vaut aussi pour une définition redonnée dans un
chapitre ultérieur, et pour une figure redondante.

Renvoyer suppose de pouvoir citer : le résultat d'origine doit donc porter un
label ([C5](communes.md#c5--labels-et-renvois)) et, dès lors qu'il est cité de
loin, réénoncer ses hypothèses ([P1](#p1--placement-des-hypothèses)). C'est le
cas typique où l'on **ajoute** un label plutôt que d'en retirer un.

## P10 — Cohérence terminologique et notationnelle

→ voir **[`communes.md` C2 et C3](communes.md#c2--cohérence-terminologique-et-notationnelle)**.

Spécifique au polycopié : c'est le document **de référence** du cours. Les
arbitrages de terminologie s'y prennent, et les transparents, TD et examens s'y
alignent — pas l'inverse.

## P11 — Conventions typographiques

→ voir **[`communes.md` C4](communes.md#c4--typographie)** pour la checklist.

**Deux règles ont changé de sens** depuis les documents d'origine, et le corpus
suit encore l'ancienne :

- **ne pas écrire `~:`** — babel-french pose l'espace lui-même. Le corpus en
  compte plus de mille, tous sans effet ;
- **`\enquote{…}`** remplace `` ``…'' ``, qui produisait des guillemets
  **anglais** dans un texte français.

Les deux se corrigent mécaniquement :

```bash
./conventions/bin/nettoyer C4 poly/            # aperçu
```

Spécifique au polycopié : la paire `\keyword{terme}` + `\index{terme}` à la
première occurrence est ce qui **peuple l'index** ([P14](#p14--structure-du-polycopié)).
Un polycopié dont l'index est vide n'a pas eu sa passe typographique.

## P12 — Labels `\ref`-ables uniquement si le résultat est cité

→ voir **[`communes.md` C5](communes.md#c5--labels-et-renvois)** : la clé qu'on
écrit est la clé qu'on référence, et le préfixe (`thm:`, `fig:`…) en fait partie.

Spécifique au polycopié : **ne pas étiqueter en masse**. Un label ne se pose que
si le résultat est cité ailleurs — ce qui est exactement le critère de
[P1](#p1--placement-des-hypothèses) pour décider où vont ses hypothèses. Les
deux règles se lisent ensemble : *poser un label, c'est déclarer que le résultat
sera lu isolément, donc s'engager à le rendre autonome.*

## P13 — Figures

- Une figure est **annoncée et référencée dans le texte avant** d'apparaître
  (« … `\cf` l'illustration Figure~\ref{…} »).
- Légende **courte et descriptive**.
- Placement `[ht!]` par défaut.
- **Une figure ne vit pas dans une boîte** `remark` / `example` : elle en casse
  le mode paragraphe (erreur `Not in outer par mode`). Elle est sortie en
  `figure` flottante — et le `\ref` qui la vise vérifié après déplacement. Même
  contrainte dans un exercice, [TD9](td.md#td9--flottants-dans-un-exercice).

Une figure peut être **l'exploitation** d'un résultat au sens de
[P4](#p4--un-résultat-qui-nest-pas-exploité-na-pas-été-posé) : montrer ce que le
théorème signifie vaut souvent mieux qu'une phrase qui le paraphrase.

## P14 — Structure du polycopié

Tout polycopié a la même ossature. Référence :
[`calcul-differentiel-edo-enseignants/poly/main.tex`](https://github.com/ocourses/calcul-differentiel-edo-enseignants/blob/main/poly/main.tex).

| Partie | Contenu | Obligatoire |
|---|---|---|
| `\frontmatter` | `\maketitle` · **Avant-propos** · `\tableofcontents` · **Notations** | oui |
| `\mainmatter` | `\part` / `\chapter`, corrections d'exercices en fin de partie | oui |
| `\begin{appendix}` | compléments, grands théorèmes | selon le cours |
| `\backmatter` | **bibliographie** · **`\printindex`** | oui |

### L'avant-propos

Un `\chapter*{Avant-propos}` court, qui dit :

- la **genèse** du document — d'où il vient, sur quel cours antérieur il s'appuie,
  qui l'a rédigé et quand ;
- les **contributions** — collègues qui ont assuré le cours, complété des
  corrigés, relu ;
- l'**assistance d'un agent conversationnel** quand il y en a eu une, en
  précisant que les modifications ont fait l'objet d'une relecture et d'une
  validation humaines ;
- **comment signaler une erreur** : lien vers les issues du dépôt public, en
  demandant page et section.

### La page de notations

Un `\chapter*{Notations}` dans `frontmatter/notations.tex`. Elle **fixe le décor
global** du cours (règle [P7](#p7--ouverture-de-chapitre-et-de-section)) une fois pour toutes, et
le corps du document n'y revient pas.

- Un paragraphe d'ouverture pose les conventions permanentes (« Sauf mention
  contraire, $(E,\norm{\cdot}_E)$ et $(F,\norm{\cdot}_F)$ désignent des espaces
  vectoriels normés sur $\R$… »).
- Puis des `\paragraph*` thématiques (ensembles de nombres, espaces et
  topologie, applications linéaires et matrices, différentiabilité…), chacun
  avec une `longtable` à deux colonnes : la notation, sa description.
- Les entrées **utilisent les macros du template** (règle
  [C3](communes.md#c3--macros-du-template-plutôt-que-du-latex-manuel)) : la page
  de notations est le premier endroit où une notation maison se voit.

Les entrées de cette page ne sont **pas** redéfinies dans le corps (règle
[C2](communes.md#c2--cohérence-terminologique-et-notationnelle)).

### L'index

`\printindex` en `\backmatter`, alimenté par les `\index{…}` posés à chaque
première occurrence (règle [P11](#p11--conventions-typographiques)). Un index
vide signale une passe typographique qui n'a pas eu lieu.
