<!-- LTeX: language=fr-FR -->

# Règles communes aux quatre supports

Ce qui vaut aussi bien dans un polycopié que sur un transparent, dans un TD ou
dans un sujet d'examen. **À lire dans tous les cas**, avant le fichier du
support concerné.

Numérotation stable : `C1`, `C2`, … (voir [`README.md`](README.md#numérotation--cest-une-api)).

---

## C1 — Langue et registre

**La langue du cours commande tout.** Elle est déclarée par l'option `lang=` du
template (`fr` par défaut, `en` pour les rares cours en anglais) et vaut pour le
texte, les intitulés **et les commentaires LaTeX** (`%`). Un cours en français a
des commentaires en français.

Registre académique, phrases complètes. Pas d'abréviation télégraphique dans un
texte suivi (« càd », « ds », « pr ») : utiliser les macros du template `\ie`,
`\cf`, `\resp`.

### « nous » et « on » : une répartition par rôle

Les deux coexistent, mais **chacun a son emploi** — ce n'est pas une alternance
au hasard.

| | Emploi | Exemples |
|---|---|---|
| **nous** | la voix de l'auteur qui **guide** le lecteur : annonce du programme, transition, renvoi | « **Nous aurons besoin**, dans la suite, de la structure d'espace complet de $\Lcal(E,F)$. » · « **Nous verrons** au chapitre suivant que… » · « **Commençons par** le cas linéaire. » |
| **on** | l'acteur du **raisonnement et du calcul** | « **On pose** $u = f(x)$. » · « **On en déduit** que… » · « **On applique** le théorème de convergence dominée. » |

Le test : si la phrase pourrait figurer dans une préface ou dans une phrase de
liaison, c'est « nous » ; si c'est une étape de démonstration, c'est « on ».

En cas de doute, ne pas trancher au jugé : vérifier l'usage dominant du cours au
`grep` (règle [C2](#c2--cohérence-terminologique-et-notationnelle)).

### Consignes : l'infinitif

Toute consigne adressée à l'étudiant — question de TD, d'examen ou d'exercice de
polycopié — s'écrit à l'**infinitif** :

> **Montrer** que la suite converge. · **Calculer** $\int_0^1 f$. · **En
> déduire** que $f$ est mesurable.

Pas d'impératif (« Montrez que… »), pas de futur impersonnel (« On montrera
que… », qui annonce au lieu de demander). Vaut aussi pour les consignes d'un
transparent.

## C2 — Cohérence terminologique et notationnelle

Devant deux formulations concurrentes (« sur $U$ » vs « dans $U$ »,
« Lyapunov » vs « Liapounov », `f'` vs `\diff f`), la procédure est en trois
temps. **Le `grep` mesure, il ne décide pas.**

### 1. Mesurer l'ampleur

```bash
# combien de fois chaque variante, et où
grep -rn -E "point d'équilibre|point de fonctionnement" --include='*.tex' . \
  | sed -E "s/.*(point d'équilibre|point de fonctionnement).*/\1/" | sort | uniq -c
```

Le comptage sert à **ne rien oublier** et à voir ce que coûte un changement. Il
ne désigne pas le bon terme : une forme minoritaire peut parfaitement être la
juste.

### 2. Les deux formes sont-elles vraiment équivalentes ?

C'est la question qui vient avant toute autre, et la réponse est souvent non.

> « Point d'équilibre » et « point de fonctionnement » ne sont **pas**
> synonymes : l'équilibre est celui du système **non contrôlé**.

Deux notions distinctes : on garde les deux mots, et on s'assure que la
**distinction est posée explicitement** dans le cours. Ce n'est pas un problème
de cohérence, c'est un point de fond.

### 3. Si elles sont équivalentes : l'auteur tranche, puis on applique partout

La décision revient à l'auteur, pas au comptage. Une fois prise, elle vaut sur
**tout le corpus** (règle « Portée » ci-dessous) — **sauf exception locale.**

### L'exception locale

**Là où le cours distingue deux notions, la notation doit les distinguer aussi**,
même si ailleurs les deux formes sont interchangeables. Une passe d'alignement
aveugle détruit le propos du passage.

Exemple, dans le cours de calcul différentiel
(`cd-application-differentiable.tex`) : le paragraphe qui compare dérivabilité et
différentiabilité en dimension 1 emploie **les deux** notations côte à côte —
$f'(x)$ pour la dérivée (un scalaire), $\diff f(x)$ pour la différentielle (une
application linéaire) — parce qu'il existe précisément pour poser
$f'(x) = \diff f(x) \cdot 1$. Remplacer l'une par l'autre viderait le passage de
son sens.

Une exception de ce genre se **signale par un commentaire LaTeX** à l'endroit
concerné, pour qu'une passe ultérieure ne la « corrige » pas.

### Noms propres

Translittération anglo-saxonne, pas la francisation phonétique :

| On écrit | Pas |
|---|---|
| Lyapunov | Liapounov, Lyapounov |
| Pontryagin | Pontriaguine |
| Tchebychev | Chebyshev *(à trancher par cours)* |

```bash
grep -rn -E "Liapounov|Lyapounov|Pontriaguine" --include='*.tex' .
```

### Portée : tout le corpus du cours

Un terme ou une notation fixée dans le **polycopié** se retrouve à l'identique
dans les transparents, les TD et les examens — **le polycopié fait référence**.

- C'est vrai **pour la notation** même quand le **contenu** circule dans l'autre
  sens : une passe d'harmonisation *slides → poly* remonte des notions absentes
  du poly, mais ce sont elles qui s'alignent sur les notations du poly, pas
  l'inverse.
- Les **annales** sont alignées elles aussi, pour la clarté du corpus.
- Ne pas redéfinir dans le corps ce qui est déjà fixé dans la page de notations,
  si le document en a une.

### Consigner la décision

Un arbitrage tranché qui n'est écrit nulle part se repose au cours suivant.
Chaque décision de terminologie va dans
`reports/<passe>/06-regles-style-relecture.md` du dépôt de cours (voir
[`methode.md`](methode.md#ce-que-le-dépôt-de-cours-garde-en-propre)).

## C3 — Macros du template plutôt que du LaTeX manuel

Utiliser systématiquement les macros dédiées : `\norm`, `\abs`, `\prodscal`,
`\enstq{}{}`, `\grandO{}` / `\petito{}`, `\intervalleff{}{}` /
`\intervalleoo{}{}`, `\fonction`, `\diff` / `\xdif`, `\diag`, `\rang`, `\trace`,
`\Im`, `\ker`, `\dot{x}` (**jamais** `x'` pour une dérivée en temps).

- Le jeu disponible dépend du module chargé par l'option `math=` du cours
  (`base`, `analysis`, `control`, `measure`). **Vérifier au `grep` dans
  `template/tex/math/` avant de supposer qu'une macro existe** — et ne jamais
  inventer une macro du template.
- Une macro absente du template : définition locale minimale et **commentée**
  dans le préambule, signalée en relecture.
- `\fonction` du template produit un `array` **nu** : ne s'utilise qu'en mode
  maths (`\[ \fonction{...} \]`).

## C4 — Typographie

Les règles ci-dessous sont celles du **français**. Un cours en anglais
(`lang=en`, règle [C1](#c1--langue-et-registre)) suit la typographie anglaise :
pas d'espace avant les deux-points, guillemets `` `` … '' `` doubles droits. Les
lignes « terme défini », « renvois » et « étapes de preuve » valent dans les deux
langues.

Checklist `grep`-able, à passer sur tout document :

| Point | Règle |
|---|---|
| Deux-points | `~:` — espace fine insécable avant (idem `\ie~`, `\cf~`) |
| Guillemets | français `` ``…'' ``, jamais `"` |
| Apostrophes | U+2019 → U+0027 dans les sources |
| Terme défini | première occurrence : `\keyword{terme}` **et** `\index{terme}` |
| Renvois | capitalisés et insécables : `Théorème~\ref{…}`, `Définition~\ref{…}`, `Figure~\ref{…}`, `Exemple~\ref{…}`, `Section~\ref{…}`, `Exercice~\ref{…}` |
| Étapes de preuve | `\newstep` pour séparer les étapes d'une preuve longue |
| Mots composés | « sous-section », « sous-espace » (trait d'union) |

## C5 — Labels et renvois

- Les boîtes à titre du template prennent **deux arguments obligatoires**,
  éventuellement vides : `\begin{theorem}{}{}`.
- **Ne poser un label que si le résultat est cité ailleurs.** Vérifier au `grep`
  les renvois réels avant d'en ajouter un ; sinon laisser `{}{}`. Ne pas ajouter
  de labels en masse « au cas où ».
- Label parlant quand il y en a un : `thm:…`, `prop:…`, `def:…`, `exa:…`,
  `chap:…`, `ssec:…`, `fig:…`.
- **Piège** : `\newtcbtheorem` préfixe déjà le compteur. Le label se donne **nu**
  (`\begin{theorem}{}{cauchy_lipschitz}`), sans re-préfixer — sinon le renvoi ne
  résout pas.
- Un document se compile sans aucune référence non résolue ni label
  multiplement défini. Un `??` dans le PDF est un bug, pas un détail.

## C6 — Listes et énumérations

- `enumerate` / `itemize` plutôt que « 1) 2) 3) » saisis à la main.
- `label=\roman*)` pour les points i), ii) d'un énoncé.
- Dans un TD ou un examen, les questions passent par `question` /
  `subquestion` (voir [`td.md`](td.md)), pas par un `enumerate` manuel.
