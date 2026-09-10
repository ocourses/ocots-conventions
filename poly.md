<!-- LTeX: language=fr-FR -->

# Règles de rédaction — polycopié

Les **13 règles**, dégagées pendant la relecture ligne à ligne de
`calcul-differentiel-edo-enseignants` puis éprouvées sur
`automatique-enseignants`. La numérotation **1 à 13 est historique et ne change
pas** : elle est citée dans les journaux de relecture et les messages de commit
de ces deux cours.

Prérequis : [`communes.md`](communes.md) (langue, notations, macros,
typographie, labels).

> **Le fil conducteur.** On n'enchaîne jamais des boîtes (`definition`,
> `theorem`, `proposition`, `corollary`, `remark`, `example`) sans texte autour.
> Chaque boîte est amenée par une phrase qui dit *pourquoi elle arrive*, et
> suivie d'une phrase qui *exploite* ce qu'elle apporte. Le lecteur ne doit
> jamais tomber sur un énoncé sans savoir ce qu'on cherche à en faire.

---

## 1. Placement des hypothèses

- **Définitions** : sortir « Soient $f\colon U \to F$… » dans le texte qui
  précède la boîte, pour l'alléger. La boîte se concentre alors sur la seule
  condition définissante.
- **Théorèmes** : toujours garder toutes les hypothèses **à l'intérieur** — un
  théorème doit rester citable isolément, sans remonter dans le texte.
- **Propositions** : selon le contexte.
  - Elle prolonge le texte qui précède (mêmes objets déjà en place) : pas besoin
    de les répéter.
  - Elle introduit un cadre réellement nouveau (nouveaux espaces, notation non
    encore utilisée) : on restitue les hypothèses dedans, comme un théorème.
- **Exception** : après une longue digression, si un lecteur peut vouloir lire la
  définition de façon isolée, on garde les hypothèses dedans même pour une
  définition.

## 2. Pas de blocs isolés ou enchaînés sans texte

- Toujours **au moins une phrase de liaison** entre deux boîtes (définition →
  remarque, remarque → proposition, proposition → corollaire…). **Jamais deux ou
  trois boîtes collées sans texte entre elles.**
- Une remarque qui n'ajoute qu'un alias de notation ou une précision mineure
  (« on note aussi… », « ne pas confondre… ») se **fond** dans la définition ou
  le résultat qui précède, plutôt que de rester une boîte à part.

## 3. Une phrase d'amorce *motivée* avant chaque boîte

La phrase qui précède une boîte ne fait pas que la jonction : elle **situe** le
résultat, dit **à quoi il sert**, ou **annonce sa forme**. Par exemple :

- « La propriété suivante est fondamentale, elle permet de calculer de
  nombreuses dérivées. »
- « Cette première forme ne requiert que la différentiabilité $k$ fois au point
  $x$ lui-même et fournit un reste seulement négligeable : c'est la version
  adaptée à l'étude locale. »
- « Le théorème suivant, qui établit la symétrie de $f''(x)$, montre que ce
  choix n'a en réalité aucune importance. »

Formulations passe-partout à éviter **seules** (« Nous avons le résultat
suivant. », « On a alors le théorème suivant. ») : acceptables en dépannage,
mais préférer une amorce qui apporte une information.

**Ne jamais laisser une phrase se jeter dans une boîte** (« On peut donc donner
la » suivi de `\begin{definition}`) : l'amorce est une phrase complète.

## 4. Une phrase de reprise *après* la boîte

Après une définition ou un résultat, une phrase **interprète** ce qu'on vient
d'obtenir et **amène la suite**.

**Proposition/théorème → corollaire** : la liaison dit *ce que la spécialisation
apporte* (« En prenant $E = \R^n$, on obtient la forme habituelle… », « Le cas
linéaire homogène en découle immédiatement… »), jamais un simple « On en déduit
le corollaire suivant. » sans contenu.

## 5. `remark` : ne pas en empiler

- Deux ou trois `remark` consécutives → **fusionner** (éventuellement en une
  `itemize`), **passer en texte courant**, ou **promouvoir en sous-section** si
  le contenu le justifie.
- Une `remark` qui n'est qu'une **convention de vocabulaire ou de notation**
  (« on parle de champ autonome si… », « on note $f(x)$ au lieu de $f(t,x)$ »)
  n'est pas un vrai aparté : elle va dans le **texte courant**, au fil de
  l'introduction des objets.
- La `remark` est réservée aux vrais apartés : mise en garde, cas limite, lien
  avec un résultat ultérieur, contre-exemple ponctuel.

## 6. `remark` vs `example`

Une boîte qui **instancie** un résultat général sur un cas concret (objets
précis substitués) est un `example`, **pas** une `remark`. Un contre-exemple
aussi : `example[Contre-exemple]`.

L'exemple est amené par une phrase qui dit **ce qu'il illustre**, **placée avant
la boîte** (« … cette inclusion est toujours stricte, `\cf` l'exemple
suivant. »).

## 7. Décor de section

Aussi appelée la « cast list » : chaque section — et souvent chaque sous-section — s'ouvre par un **court
paragraphe qui (re)pose les objets courants** avant la première boîte :

> Soient $(E,\norm{\cdot}_E)$ et $(F,\norm{\cdot}_F)$ deux espaces vectoriels
> normés, $U$ un ouvert de $E$, une application $f \colon U \to F$ et un point
> $x \in U$.

ou, si le décor est déjà en place plus haut : « Rappelons que $f \colon U
\subset E \to F$ désigne une application et $U$ un ouvert de $E$. »

La **liste des objets courants est propre à chaque cours** : elle se consigne
dans `reports/<passe>/06-regles-style-relecture.md` du dépôt de cours, en
cohérence avec la page de notations (`frontmatter/notations.tex`) quand elle
existe. Ne pas redéfinir dans le corps ce qui y est déjà fixé.

## 8. Ne pas répéter une hypothèse

Si une hypothèse (« $E$ Hilbert », « $f$ de classe $\xCn{1}$ ») sert à plusieurs
conséquences dans un même exemple ou une même preuve, la poser **une seule
fois**, à l'endroit qui couvre toutes ses conséquences — jamais répétée à chaque
usage.

## 9. Ne pas re-dériver un cas particulier d'un résultat déjà écrit

Si un second énoncé n'est que la **restriction** ou un **cas particulier** d'un
résultat déjà donné plus haut, y **renvoyer** plutôt que de le réafficher ou de
le redémontrer en entier. Cela vaut aussi pour une définition redonnée dans un
chapitre ultérieur, et pour une figure redondante.

## 10. Cohérence terminologique et notationnelle

→ voir **[`communes.md` C2 et C3](communes.md#c2--cohérence-terminologique-et-notationnelle)**.

Spécifique au polycopié : c'est le document **de référence** du cours. Les
arbitrages de terminologie s'y prennent, et les transparents, TD et examens s'y
alignent — pas l'inverse.

## 11. Conventions typographiques

→ voir **[`communes.md` C4](communes.md#c4--typographie)** pour la checklist.

Spécifique au polycopié : la paire `\keyword{terme}` + `\index{terme}` à la
première occurrence est ce qui **peuple l'index**. Un polycopié dont l'index est
vide n'a pas eu sa passe typographique.

## 12. Labels `\ref`-ables uniquement si le résultat est cité

→ voir **[`communes.md` C5](communes.md#c5--labels-et-renvois)**.

## 13. Figures

- Une figure est **annoncée et référencée dans le texte avant** d'apparaître
  (« … `\cf` l'illustration Figure~\ref{…} »).
- Légende **courte et descriptive**.
- Placement `[ht!]` par défaut.
- **Une figure ne vit pas dans une boîte** `remark` / `example` : elle en casse
  le mode paragraphe (erreur `Not in outer par mode`). Elle est sortie en
  `figure` flottante — et le `\ref` qui la vise vérifié après déplacement.

## 14. Structure du polycopié

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
global** du cours (règle 7) une fois pour toutes, et le corps du document n'y
revient pas.

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
première occurrence (règle 11). Un index vide signale une passe typographique
qui n'a pas eu lieu.
