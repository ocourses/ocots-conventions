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
