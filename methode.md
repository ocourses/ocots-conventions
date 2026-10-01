<!-- LTeX: language=fr-FR -->

# Méthode — conduire une passe de relecture

Comment se déroule une relecture, quel qu'en soit le support. Éprouvée sur
`calcul-differentiel-edo-enseignants` puis `automatique-enseignants`.

---

## Le périmètre se déclare avant de commencer

Une passe est **soit de forme, soit de fond**, jamais les deux mélangées.

- **Forme** : amorces, transitions, placement des boîtes, typographie, cohérence
  des notations. Pas d'ajout de contenu mathématique.
- **Fond** : preuves manquantes ou fausses, énoncés incorrects, restructuration.
  Décidé **avec l'auteur**, point par point, avant d'écrire une ligne.

Ce qu'on repère hors périmètre se **note**, ne se corrige pas. C'est ce qui
alimente la passe suivante.

## Le travail se fait par salves

Une salve = un fichier, ou un groupe cohérent de points. Pour chacune :

1. présenter l'analyse et **une proposition de diff concrète**, attendre
   confirmation avant d'éditer (pour une relecture pilotée par l'auteur) ;
2. éditer ;
3. **vérifier** : recompiler, puis les trouvailles de la salve seule (voir
   [plus bas](#la-vérification-suit-chaque-salve)) ;
4. commiter la **source lisible** ;
5. en fin de salve seulement, un commit `build: recompilation` si le PDF est
   suivi par git.

**Ne jamais garder plusieurs fichiers non commités en parallèle.** Sur un
document de plusieurs centaines de lignes, c'est la seule façon de ne rien
perdre si la session est coupée.

## Les warnings de départ se relèvent d'abord

Avant la première édition : compiler et **noter les warnings existants**
(la « baseline »). Sans ça, impossible de dire si un warning est nouveau.

Relever aussi le compte des règles outillées : il donne l'ampleur du travail et
permet de l'annoncer.

```bash
./conventions/bin/verifier poly/ 2>&1 >/dev/null   # les comptes, par règle
```

Chaque salve annonce ensuite ce qu'elle change des deux côtés : warnings
inchangés ou résorbés, compte de règle en baisse. La dernière salve vise **zéro
warning**.

Les comptes ne certifient rien — l'outil rate des choses et en signale d'autres
à tort (voir [`README.md`](README.md#vérifier-et-corriger)). Ils mesurent
une ampleur et suivent une tendance.

## Un nettoyage mécanique est sa propre salve

`bin/nettoyer` corrige d'un coup ce qui l'est — les `~:` inutiles, les
guillemets. Sur un polycopié, cela touche des dizaines de fichiers pour un rendu
identique. **Ce genre de correction ne se mêle jamais à une passe de fond** :
elle noierait le diff qu'il faut relire.

Elle se fait donc seule, dans sa propre salve et sa propre PR, avec la
vérification qui va avec : recompiler, et **comparer le texte du PDF avant et
après**. Un nettoyage typographique ne doit rien changer au rendu.

```bash
pdftotext avant.pdf avant.txt && pdftotext apres.pdf apres.txt && diff avant.txt apres.txt
```

## La vérification suit chaque salve

Après chaque salve, avant de la commiter :

1. **compiler** avec `latexmk` : exit 0, et le log **sans** référence non
   résolue ni label multiplement défini, warnings comparés à la baseline ;
2. **vérifier les trouvailles que la salve a introduites**, et elles
   seules :

   ```bash
   ./conventions/bin/ocots-lint verifier --nouvelles origin/main <périmètre>
   ```

   Les trouvailles déjà présentes sur `origin/main` ne sont pas celles de la
   salve : elles relèvent de leur propre passe (ou d'une issue
   `[conventions]`). Une trouvaille nouvelle se corrige, ou, si elle est
   jugée correcte, s'exempte dans la source avec sa raison (README
   d'`ocots-lint`, « Exempter une trouvaille justifiée ») ;
3. **relire le diff** et ne garder que les changements demandés ;
4. ne pas ajouter d'artefact de compilation (`*.aux`, `build/`…).

Zéro trouvaille nouvelle ne veut pas dire règle respectée : l'outil ne
couvre qu'une partie des règles (`./conventions/bin/ocots-lint couverture`),
le reste se relit.

## Le suivi est le plan, le journal et le bilan

Un fichier unique par passe, dans le dépôt de cours :

```text
reports/<nom-de-la-passe>/
  00-suivi.md                      cap retenu, méthode, journal par salve, bilan
  06-regles-style-relecture.md     ce qui est propre au cours (voir plus bas)
  0X-<sujet>.md                    documents de cadrage produits en route
```

`00-suivi.md` porte :

- la **version des conventions appliquée**, en tête — sans elle, une citation de
  règle n'est plus interprétable dans quelques mois :

  ```text
  Conventions : ocots-conventions v1.0.0 (commit 1a2b3c4)
  ```

  ```bash
  git -C conventions describe --tags --always
  ```

- le **cap retenu** — ce qu'on améliore, ce qu'on ne touche pas, décidé avec
  l'auteur et daté ;
- la **baseline** des warnings ;
- le **journal**, une entrée par salve, avec les hash de commit ;
- l'**état des documents** (tableau : fichier, contenu, état) ;
- le **bilan** : ce qui a changé, ce qui est conservé en renvoi, et surtout
  **ce qui reste à vérifier à la main**.

Travail sur une branche dédiée, **PR ouverte tôt et laissée en Draft** : le fond
mathématique demande une relecture humaine avant fusion.

## Une relecture de jugement se prépare avec l'extraction

P1, P3, P4 et P7 ne se vérifient pas : elles se jugent. `ocots-lint extraire`
(à partir d'ocots-lint v0.8.0) en prépare la relecture, sans rien juger :

```bash
./conventions/bin/ocots-lint extraire poly/<chapitre>.tex > extraction.json
```

- **les boîtes**, chacune avec son empreinte, sa famille, sa section, ses
  citations (`citations.ailleurs` : citée d'un autre fichier, donc lue
  isolément — P1), son amorce et ce qui la précède (P3), sa preuve, sa
  reprise et ce qui la suit (P4) ;
- **la carte des sections** : ouverture (texte, introduction de chapitre,
  `\minitoc`), contenu dans l'ordre, blocs `assumption`, ce qui termine la
  section (P7, et P4 en fin de section).

L'extraction garantit que **rien n'est sauté** : le rapport juge chaque
boîte et chaque section qu'elle liste, ni plus ni moins, et ne relit dans la
source que ce qu'il faut (le corps d'un résultat pour P1). Le format est
décrit par le schéma `extraire-1.schema.json` d'ocots-lint. Un agent suit le
rôle `judgment-reviewer` d'`ocourses/agents`, qui applique ce qui précède.

P1 et P4 ne portent que sur les **résultats** (famille `resultat` de
l'extraction) : un exemple qui suit un résultat *est* son exploitation, et
une section qui finit sur un exemple n'est pas un point P4.

## Format d'un rapport de relecture

Quand la passe produit un rapport plutôt que des corrections, trois niveaux :

| Niveau | Contenu |
|---|---|
| **Bloquant** | erreurs de fond, énoncés faux, incohérences |
| **Important** | imprécisions, notations flottantes, renvois cassés |
| **Mineur** | typographie, style, présentation |

Chaque point : **localisation** (`fichier:ligne` ou nom d'exercice),
**description**, **suggestion**. Un relecteur liste les erreurs de fond, il ne
les corrige pas — sauf les coquilles purement typographiques, dans un commit
séparé et clairement nommé.

## Ce que le dépôt de cours garde en propre

`reports/<passe>/06-regles-style-relecture.md` ne recopie pas les règles : il
renvoie ici et ne porte que les adaptations du cours —

- le **décor de section** ([P7](poly.md#p7--ouverture-de-chapitre-et-de-section)) : la liste des
  objets courants ;
- les **arbitrages de terminologie** tranchés pour ce cours ;
- les **cas concrets repérés** (`fichier:ligne`), qui sont la matière des salves.

## Commits

Conventional Commits **en français**, atomiques, un par groupe de points :
`type(scope): description` (`feat`, `fix`, `refactor`, `style`, `docs`,
`chore`). Jamais `git push --force`, jamais `git add -A` aveugle. On ne commite
pas d'artefact de compilation (`*.aux`, `build/`) — le PDF seulement s'il est
déjà suivi, et dans son propre commit de fin de salve.
