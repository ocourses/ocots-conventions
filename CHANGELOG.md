<!-- LTeX: language=fr-FR -->

# Journal des versions

Une passe de relecture **épingle la version** de ces conventions
(voir [`README.md`](README.md#citer-une-règle--épingler-la-version)). Ce fichier
dit ce qui a changé d'une version à l'autre — en particulier tout renommage ou
réordonnancement de règle, pour qu'une trace ancienne reste relisible.

```bash
git -C conventions describe --tags --always
```

---

## Non publié

---

## v2.3.0 — 2026-09-28

Version mineure : les outils changent, pas les identifiants de règle.

- `ocots-lint` passe à
  [`v0.4.0`](https://github.com/ocourses/ocots-lint/releases/tag/v0.4.0) :
  `synchroniser` ouvre une issue `[nettoyer] <fichier>` (label
  `conventions-mecanique`) pour ce que `nettoyer` sait corriger ; la file
  des agents la traite sans modèle (`ocourses/agents#31`). Un cours qui
  monte à cette version doit avoir le workflow `agent-nettoyer.yml`.

---

## v2.2.0 — 2026-09-28

Version mineure : les outils changent, pas les identifiants de règle.

### Outils

- **Nouveau relais générique `bin/ocots-lint`**, seul endroit où la version
  d'`ocots-lint` est épinglée : `./conventions/bin/ocots-lint <commande>`
  donne accès à `synchroniser`, `exempter`, `comparer`… `bin/verifier` et
  `bin/nettoyer` passent par lui.
- `ocots-lint` passe de `v0.2.0` à
  [`v0.3.0`](https://github.com/ocourses/ocots-lint/releases/tag/v0.3.0) :
  empreintes, contrat JSON, exemptions posées par `exempter`, issues par
  `synchroniser`, `--nouvelles`, `comparer`. Sortie texte de `verifier`
  inchangée.

---

## v2.1.0 — 2026-09-28

Version mineure : les outils changent, pas les identifiants de règle.

### Outils

- **`bin/verifier` et `bin/nettoyer` deviennent des relais** vers
  [`ocots-lint`](https://github.com/ocourses/ocots-lint) `v0.2.0`, lancé par
  `uvx`. Sorties identiques à l'ancien script sur un cours réel ; prérequis :
  `uv` (sortie `2` sans lui). `OCOTS_LINT` remplace la commande (clone
  local). Le code Python d'origine vit désormais dans `ocots-lint`.
- CI : les relais sont testés à chaque PR.

---

## v2.0.0 — 2026-09-28

Version majeure : les identifiants `SL2` à `SL7` ont changé de sens (voir
[Versions](README.md#versions)).

### Rupture : renumérotation des transparents

Un `SL2` a été intercalé le soir même du tag `v1.0.0` (`d690b45`), sans être
consigné ici. Une trace qui cite `SL2` à `SL7` sous `v1.0.0` se lit avec
cette table :

| `v1.0.0` | Depuis |
|---|---|
| `SL1` — le polycopié est la référence | `SL1` (inchangé) |
| — | **`SL2`** — viser une numérotation identique au polycopié (nouvelle) |
| `SL2` — une idée par diapositive | `SL3` |
| `SL3` — le transparent n'est pas le polycopié | `SL4` |
| `SL4` — pas de preuve longue en transparent | `SL5` — preuves en transparent : un choix de cours, une seule bonne façon |
| `SL5` — `\pause` : progression, pas décoration | `SL6` |
| `SL6` — figures lisibles en projection | `SL7` |
| `SL7` — le thème est un réglage global | `SL8` |

Même identifiant, titre et portée revus (pas de renumérotation) :

| Identifiant | `v1.0.0` | Depuis |
|---|---|---|
| `P4` | une phrase de reprise *après* la boîte | un résultat qui n'est pas exploité n'a pas été posé |
| `P5` | `remark` : ne pas en empiler | ce qu'est une `remark` |
| `P7` | décor de section | ouverture de chapitre et de section |
| `TD5` | nommer le résultat du cours mobilisé | citer le résultat du cours mobilisé, pas le redémontrer |

Dans `template.md`, les principes de nommage passent de `P1`–`P6` à `N1`–`N6`,
pour ne plus entrer en collision avec les identifiants du polycopié.

### Outils

- **`bin/verifier`** (nouveau) : règles mécaniques `P2`, `P3`, `P5`, `C4`,
  `C6`, sortie `1` en cas d'infraction. Les commentaires LaTeX sont masqués
  avant `P2`, `P5` et `C4` ; `P3` ne s'applique pas aux fichiers `slides/`.
  Il est porté à l'identique dans
  [`ocots-lint`](https://github.com/ocourses/ocots-lint) `v0.1.0`, qui en
  reprend le développement.
- **`bin/nettoyer`** (nouveau) : corrections mécaniques dont l'équivalence est
  vérifiée (`~:`, guillemets), aperçu par défaut, refus si le document n'a pas
  les prérequis.
- **`bin/liens`** (nouveau) : contrôle les renvois internes entre fichiers
  Markdown.

### `verifier` : `C6` et `--mesure`

- **Nouvelle règle `C6`** (bloquante) :
  - une `proof` ou un `example` qui se termine par une liste ou une équation hors texte sans `\qedhere` ;
  - un `\vspace` ou un `\medskip` collé à un `\footnotetext`, qui compense un écart que le template ne produit plus (ocots-latex-template#51).
- **Nouvelle option `--mesure`** : elle compte, sans jamais échouer, des motifs sûrs mais encore trop répandus pour bloquer :
  - C3 : ancienne syntaxe `{titre}{label}`, `\emph{\textbf{…}}`, `{{…}}` ;
  - C4 : étapes numérotées à la main ;
  - C1 : « t.q. » ;
  - C6 : espaces verticaux manuels.

### `poly.md`

- `P1` : un critère unique — un résultat cité de loin réénonce ses hypothèses.
- `P3` : table de cinq questions auxquelles l'amorce peut répondre ; ce qui est
  fautif est la phrase qui s'arrête à l'annonce.
- `P4` : réécrite autour de l'exploitation d'un résultat (exemple,
  discussion des hypothèses, contre-exemple), après l'unité énoncé + preuve.
- `P5` : réécrite autour de ce qu'est une remarque (mise en avant,
  optionnelle) ; quatre remarques d'affilée sont un signal.
- `P2` : exceptions pour une série d'exercices et pour l'entrée en remarque.
- `P7` : ossature de chapitre, outils d'ouverture de section, préfixe `hyp:`.
- `P8`, `P9`, `P11` à `P14` : rattachements, signaux mesurables et cas
  concrets mesurés sur les cours ; `P12` tranche pour le critère strict.
- `P13` : table des environnements où un flottant casse la compilation.
- `P7` : l'introduction de chapitre s'écrit dans `chapterintro` (la note « en attendant la révision du template » est retirée, le chantier 3 étant appliqué). Le saut de page après l'introduction n'est plus systématique : il ne se met que si la première section commencerait sinon sur la page de titre du chapitre.

### `slides.md`

- `SL1` : table de ce que les transparents peuvent retirer (preuves,
  corrigés) sans décaler la numérotation.
- `SL4`, `SL5` : grep corrigé, `SL5` reformulée sur le choix de cours ;
  `SL6` à `SL8` confirmées.
- `SL3` : l'unité est l'idée, pas la boîte. Deux objets courts qui forment une même idée (une définition et celle qui s'en sert, deux exemples du même phénomène) vont sur la même diapositive, séparés par un `\pause`, s'ils tiennent sans compression (#14).
- `SL6` : le second objet d'une paire de `SL3` devient l'emploi type de `\pause` ; vérifier la numérotation des boîtes sur chaque étape.
- **Nouvelle règle `SL9`** : aérer les mathématiques quand la diapositive a de la place (formules en display, étapes sur des lignes distinctes), sans rallonger le texte. Aucun renumérotage.
- **Nouvelle règle `SL10`** : au moins un temps de manipulation par section (« Exercice : le démontrer. » ou « Qu'allons-nous montrer ? »), une seule idée, réponse après un temps de réflexion ; `SL6` le cite comme second emploi type de `\pause`.

### `communes.md`

- `C2` : `\diff` devient `\dif`, nom actuel du template.
- `C3` : renvoie vers `notations.md` du template plutôt que lister les noms.
- `C5` : une règle unique — la clé qu'on écrit est la clé qu'on référence ;
  table des préfixes.
- `C6` : table de choix des listes, espacement réservé au support.
- `C6` : deux nouvelles sous-sections, sur `\qedhere` en fin de liste ou d'équation et sur la note de bas de page dans un énoncé.

### `td.md`, `exam.md`

- `TD1` à `TD9` : relues, cas concrets mesurés ; `TD4` revue sur l'équilibre
  entre thèmes ; `TD5` sépare le `\ref` (base) du nom (bonus).
- `EX1` à `EX7` : mesurées sur quatorze sujets.

### `template.md`

- Chantiers 4, 5 et 7 proposés ; compteur partagé décidé pour le chantier 1 ;
  un état par chantier.

---

## v1.0.0

Première version. Rassemble des règles jusque-là dupliquées à la main dans
chaque cours et les étend aux quatre supports.

### Correspondance avec la numérotation antérieure

Les journaux de relecture de `calcul-differentiel-edo-enseignants`
(`reports/poly-update-2026/`) et `automatique-enseignants`
(`reports/poly-relecture-2026/`) citent les règles du polycopié par un **nombre
nu** : « règle 5 », « règles 3, 4, 13 ».

**La correspondance est l'identité : la règle *n* est devenue P*n*.** Le
renommage n'a rien réordonné.

| Antérieur | Ici |
|---|---|
| règle 1 … règle 13 | `P1` … `P13` |

Deux règles renvoient désormais au socle plutôt que de répéter son contenu :

| Antérieur | Ici |
|---|---|
| règle 10 (cohérence terminologique) | `P10` → `C2` et `C3` |
| règle 11 (typographie) | `P11` → `C4` |
| règle 12 (labels) | `P12` → `C5` |

### Ajouté

- `communes.md` — socle transverse `C1`–`C6`.
- `P14` — structure du polycopié : avant-propos, page de notations, index,
  bibliographie.
- `td.md` (`TD1`–`TD9`), `slides.md` (`SL1`–`SL7`), `exam.md` (`EX1`–`EX7`).
- `methode.md` — conduite d'une passe.
- `template.md` — les révisions demandées au template LaTeX (proposition).

### Corrigé par rapport aux documents d'origine

- « une vraie aparté » → « un vrai aparté » (*aparté* est masculin).
- `C2` ne dit plus « aligner les exceptions sur la majorité » : le `grep` mesure,
  l'auteur tranche, et une notation minoritaire peut être la juste.
- `C3` : `\Im` n'existe pas dans le template (c'est le `\Im` de LaTeX, partie
  imaginaire) ; la macro est `\im`. `\ker` → `\Ker`.
- `C4` : `` ``…'' `` produit des guillemets **anglais**, pas français. La règle
  disait le contraire.
- `C4` : la règle du `~:` est inversée — babel-french gère seul l'espace avant
  les deux-points, le `~` est sans effet.
