<!-- LTeX: language=fr-FR -->

# Règles de rédaction — sites web des cours

Les pages interactives d'un cours (`site/`) : l'accueil, les pages
d'illustration, et les pages propres au cours (le simulateur d'automatique). Le
site est construit sur le socle
[`ocots-site`](https://github.com/ocourses/ocots-site), monté en sous-module
dans `site/socle/` : gabarits, feuilles de style, briques et tests communs.

Prérequis : [`communes.md`](communes.md), pour la langue
([C1](communes.md#c1--langue-et-registre)) et les notations
([C2](communes.md#c2--cohérence-terminologique-et-notationnelle)). Les règles
propres au LaTeX (macros, labels, listes) ne s'appliquent pas ici.

Identifiants : `WEB1`, `WEB2`, … (voir [`README.md`](README.md#citer-une-règle--épingler-la-version)).

**Version du socle.** Ces règles décrivent `ocots-site` **v0.1.0** et
au-delà. Un cours cite la version des conventions **et** celle du socle
(`git -C site/socle describe --tags`) : une règle qui nomme une brique
(`champ()`, `verifieCours`) s'entend dans cette version du socle.

**Chaque règle dit ce qui la vérifie** : un test du socle (il tourne dans
chaque cours, côté enseignant comme côté étudiant), un test du cours, un
outil, ou la relecture.

**Ce qui reste dans le cours.** Le `site/CONVENTIONS.md` d'un cours ne garde
que ses arbitrages : la page de référence, le tableau des rôles de ses couleurs
([WEB11](#web11--une-couleur-un-rôle-défini-par-le-cours)), ses notations, ses
briques propres et l'ordre de son catalogue. Il renvoie ici pour le reste.

---

## WEB1 — Les gabarits du socle, sans les modifier

`index.html` et `illustration.html` sont des **copies exactes** des modèles du
socle (`socle/modeles/`). Ils lient `socle/css/theme.css`,
`socle/css/style.css`, puis `css/cours.css`, et démarrent la page avec
`site.config.js`. Ce qu'un cours ajoute passe par les points d'extension du
socle (modules d'illustration, `css/cours.css`, pages à lui, `pagesDuTheme`,
greffe sur une section `#theme-<id>` de l'accueil), jamais par une copie
modifiée d'un fichier du socle. Seule exception : un cours qui greffe un bloc
sur l'accueil (le simulateur d'automatique) garde son `index.html`, construit
sur le modèle, et ne vérifie que `illustration.html`.

Ce que le gabarit garantit à chaque page d'illustration :

- une boîte à onglets à gauche (« Ce que dit le cours », « À essayer ») et
  l'illustration à droite ; l'onglet « À essayer » est masqué s'il n'y a pas
  d'essai ;
- un en-tête sur deux lignes : le fil d'Ariane « ← Accueil · thème », puis le
  titre suivi de la source, tirés du catalogue ;
- « ← Page précédente » et « Page suivante → », dans l'ordre de lecture du
  cours ([WEB16](#web16--le-catalogue-suit-lordre-du-poly)) ; le retour à
  l'accueil revient à la carte de la page quittée, pas en haut.

Une page propre au cours (le simulateur) suit les mêmes conventions de
navigation : « ← Accueil » s'écrit ainsi sur **toutes** les pages.

Vérifié par : `verifieModeles` (test du cours) ; `verifieCours` pour le
catalogue et le parcours ; tests du socle sur les modèles réels.

## WEB2 — Tout tient dans la fenêtre sur grand écran

Au-delà de 900 px de large et 620 px de haut, la page prend la hauteur de la
fenêtre (1000 px au plus). La boîte de gauche a la hauteur de la colonne de
droite, et **les cartes ne défilent pas**. Si le contenu ne tient pas, on
réduit les figures, pas la lisibilité du texte.

- Sous 900 px de large, une seule colonne, qui défile normalement, sans
  `100vh` rigide ; l'illustration passe avant le texte du cours.
- Entre 900 et 1200 px, si le contenu ne tient plus, seule la zone de
  l'illustration défile, jamais la page entière.
- Les actions de l'en-tête restent sur une ligne ; c'est le titre qui cède.

La fenêtre de contrôle est **1504 × 856** ; on vérifie aussi 1504 × 600
(fenêtre basse) et 390 × 844 (téléphone).

Vérifié par : la feuille du socle ; relecture à ces trois tailles
([WEB20](#web20--vérifier-avant-de-livrer)) ; `scripts/styles-calcules.py` du
socle pour comparer deux versions d'un site.

## WEB3 — Les canevas remplissent leur carte, et ce sont eux qui cèdent

Un canevas se crée par `toile()` : un canevas carré (repère orthonormé) garde
ses proportions et se centre, les autres s'étirent. On ne fixe **jamais** de
hauteur en pixels à un canevas. Dans une carte, un bloc de texte ou de
formules garde sa hauteur, et c'est le canevas voisin qui rétrécit ; une carte
de colonne ne descend pas sous la hauteur de son texte.

Vérifié par : relecture ; lint possible (hauteur en pixels sur un canevas dans
`css/cours.css` ou dans un module).

## WEB4 — La mise en page par les classes du socle

On compose avec les dispositions du socle : `.grille-2` (deux cartes côte à
côte), `.deux-colonnes` (deux cartes empilées à gauche, une sur toute la
hauteur à droite), la rangée `principale` (deux fois plus haute). Un contenu
annexe va dans un bloc `repliable()`, fermé par défaut, dont l'état est une
préférence. Une disposition qu'un seul cours emploie est dans son
`css/cours.css`, et le `CONVENTIONS.md` du cours la décrit.

Un **choix qui modifie toute la page** (le système, l'étape, la forme) se fait
par des boutons radio dans une barre d'outils au-dessus des cartes
(`barreOutils`, `choixRadios`) : jamais par de gros boutons, ni dans une carte
de texte qu'il modifie.

Vérifié par : contrat JS ↔ CSS du socle (toute classe posée par une brique
existe dans sa feuille) ; relecture.

## WEB5 — Une mise en page qui ne bouge pas quand une valeur change

Un texte qui affiche une valeur modifiée au glisser ou pendant une animation ne
doit ni changer de largeur ni sauter de ligne, et la figure voisine ne doit pas
changer de taille.

- **Un nombre garde sa largeur pendant le geste** : ni le « − » qui apparaît,
  ni une décimale ou un chiffre de plus ne doivent faire bouger ce qui le
  suit. La forme dépend de ce que fait la grandeur :

  | La grandeur… | Forme | Fonctions de `format.js` |
  |---|---|---|
  | varie dans une plage connue (réglage, aire, intégrale) | virgule fixe | `latexNombreFixe`, `latexPositifFixe`, `latexComplexeFixe`, `latexMatriceFixe` |
  | traverse plusieurs ordres de grandeur (erreur, écart qui tend vers 0) | notation scientifique **à largeur fixe** : signe réservé, mantisse à décimales fixes, exposant à largeur fixe | `nombreScientifique` (texte) ; `latexScientifiqueFixe` (LaTeX, socle v0.3.0) |
  | ne change pas pendant le geste | la forme la plus courte | `latexNombre`, `texte`, `latexNombreFinal` |
  | est une étiquette d'axe | mêmes décimales pour toutes ; scientifique aux extrêmes | `etiquettesAxe` |
  | s'écrit dans un journal (ligne ajoutée, jamais réécrite) | libre | `texte`, `nombreCourt` |

  Le seuil entre virgule fixe et notation scientifique est celui du socle :
  moins de 10⁻³ ou au-delà de 10⁵. Une notation scientifique dont la largeur
  varie (`latexScientifique`, `nombreCourt`) ne convient pas à une valeur qui
  bouge.
- Quand la phrase elle-même change (nom du cas, verdict), les nombres vont
  dans une formule ou un tableau à part, de largeur fixe.
- Un texte qui change avec les réglages a sa **hauteur réservée** (`.verdict`,
  `.bloc-maths`, rangée `hauteur-fixe`).
- Un élément qui n'a de sens que dans un mode reste en place, grisé ou vide.
- Un texte ou un tableau qui change se construit à part, puis remplace l'ancien
  d'un coup (`replaceChildren`) : le vider d'abord étire le canevas voisin.

Vérifié par : relecture, au glisser ; test du socle sur la largeur des
six fonctions « fixes » du tableau (`tests/format.test.js`, socle v0.3.0).

## WEB6 — Un seul corps de texte, des formules du même corps

Le cours, les essais, les notes et les verdicts sont au même corps
(`--taille-texte`, 0,9 rem), dans la police du texte ; les formules KaTeX ont
le même corps partout (1,1 em), simulateur compris. On ne fixe pas de taille de
police dans un module.

Vérifié par : la feuille du socle ; lint possible (`font-size` dans
`css/cours.css`).

## WEB7 — « Ce que dit le cours » énonce, sans démontrer

Le rappel énonce le résultat **du poly**, avec ses notations, sans
démonstration. Une énumération de cas est une liste à puces (un élément de
`rappel` qui est lui-même un tableau). Une formule qu'on retient va **seule
sur sa ligne**, centrée (`$$…$$`), entre deux phrases qui l'introduisent et la
commentent ; les petites formules de commentaire restent dans le texte.

Vérifié par : relecture.

## WEB8 — Les notations du poly, les opérateurs en romain

Les notations sont celles du poly ou du TD cité dans la source de la page
([C2](communes.md#c2--cohérence-terminologique-et-notationnelle)) : on suit la
source affichée sous le titre. Tout opérateur s'écrit en romain, jamais en
italique (`\det`, `\log`, `\operatorname{tr}`, `\mathrm d\mu`…), dans le texte
comme sur les axes, où l'on passe des segments
`[['tr ', false], ['A', true]]`.

Vérifié par : relecture ; les notations propres à un cours sont dans son
`CONVENTIONS.md`.

## WEB9 — Typographie française, à l'affichage

Le site affiche ses textes tels quels, sans passer par LaTeX : la typographie
est celle du texte affiché.

- Espaces insécables avant « : », « ; », « ! », « ? » et après « « » :
  `typographie()` et `rendTexte()` les posent. La ponctuation qui suit une
  formule reste collée à elle.
- Guillemets « … » et apostrophe **’** dans les textes du site (catalogue,
  modules, glossaire). C'est l'inverse des sources LaTeX
  ([C4](communes.md#c4--typographie)), où l'apostrophe est `'`.
- Nombres à la française : `2{,}5` dans une formule (`latexNombre`), jamais une
  virgule nue.

Vérifié par : `typographie`, `rendTexte` et les fonctions de `format.js` ;
lint possible (apostrophe droite dans les textes d'un module ou du catalogue).

## WEB10 — Une figure se lit avec sa légende en bulle

Toute figure qui a plus de deux éléments à expliquer a un bouton « i » en haut
à droite de sa carte (`boutonInfo`) : une légende avec des pastilles (ligne,
tirets, point, cercle, flèche, aplat), ce que représente chaque tracé et ce
qu'on peut faire à la souris. Pas de longue phrase de légende sous la figure.

Vérifié par : relecture.

## WEB11 — Une couleur, un rôle défini par le cours

Aucune couleur n'est écrite en dur : tout passe par les variables de
`theme.css`, qui ont une valeur claire et une valeur sombre. Les canevas les
lisent par `couleurs()` et se redessinent au changement de thème
(`outils.surTheme`).

- Chaque variable a **un seul rôle**, le même sur toutes les pages d'un cours.
  Le cours en donne le tableau dans son `CONVENTIONS.md` (« Condition
  initiale : `--perturbation` », « Limite : `--equilibre` »…).
- `--alerte` ne sert qu'à ce qui échoue ou est instable, jamais à distinguer
  des courbes. Des séries comparées prennent `--serie-1` à `--serie-4`, qui
  n'ont pas d'autre rôle.
- **Rappels croisés** : un même objet a la même couleur dans toutes les figures
  de la page.
- Les boîtes n'ont ni liseré coloré ni fond teinté ; les bulles ont
  `--fond-bulle` et `--bord-bulle`.

Vérifié par : contrat JS ↔ CSS du socle (variables lues par `theme.js`) ; lint
possible (couleur littérale dans `css/cours.css` ou dans un module).

## WEB12 — Le glossaire du poly, déclaré par chaque page

Les termes définis dans le poly sont dans le glossaire du cours
(`glossaire` de `site.config.js`), avec la définition reprise au plus près du
poly et sa source. Le moteur est celui du socle.

- Une page déclare les termes qu'elle emploie dans `termes` ; leur première
  occurrence dans le cours et dans les essais est soulignée, avec la
  définition en infobulle. Un texte construit par l'illustration passe par
  `outils.glossaire(element, termes)`.
- On ajoute au glossaire tout terme du poly employé par une page, plutôt que de
  le redéfinir dans le texte. Un terme absent du poly s'explique par
  `infobulle()`, sans entrer au glossaire.

Vérifié par : `verifieCours` (chaque terme déclaré est au glossaire).

## WEB13 — Deux mémoires : l'URL pour l'expérience, le navigateur pour le confort

Chaque réglage va dans l'une des deux, et dans une seule.

- **L'URL** (`etat` du module) : ce qu'on partage en copiant l'adresse. Les
  paramètres mathématiques, le cas choisi, le point choisi. Un aller-retour par
  l'URL ne change pas le résultat (10 chiffres significatifs).
- **Le navigateur** (`preferences` du module) : le confort de chaque lecteur.
  Le mode d'animation, les blocs repliés, l'onglet ouvert. La page marche sans
  stockage (navigation privée). Le zoom d'une courbe n'est sauvé nulle part.
- Une valeur lue d'un mauvais type, non finie ou hors de `choix` est remplacée
  par la valeur par défaut : un bouton radio est toujours coché.
- Tout l'état d'une page est dans `outils.etat` : « Réinitialiser » et les
  essais passent par `applique(etat)`, qui doit tout redessiner.

Vérifié par : tests du socle (`lisEtat`, `versRecherche`, `Preferences`) ;
`verifieCours` (types des réglages et des préférences des essais) ; relecture
pour l'état local oublié.

## WEB14 — Plusieurs moyens de régler une valeur, dont le clavier

Toute grandeur réglable passe par `champ()` : un curseur, des boutons ▲ ▼ (un
pas rond, qui se répète maintenu) et la saisie au clavier (↑ ↓ d'un pas,
PageHaut, PageBas ou Maj pour dix pas). Une boîte de nombre seule reçoit les
mêmes boutons par `ajoutePas()`.

- **Un geste dont l'effet ne se voit pas sur la figure est dit.** Une phrase
  sous le canevas, dans sa carte, donne la consigne tant qu'aucun geste n'a eu
  lieu, puis dit ce que le dernier geste a fait (« Le point 4 rejoint le bloc
  {4, 5}. »). Sa place est réservée ([WEB5](#web5--une-mise-en-page-qui-ne-bouge-pas-quand-une-valeur-change)),
  et la cible se dit par son contenu, pas par son numéro. Ce n'est pas la
  légende en bulle ([WEB10](#web10--une-figure-se-lit-avec-sa-légende-en-bulle)) :
  elle dit ce qu'on *peut* faire, la phrase dit ce qu'on *vient* de faire. Un
  clic dont l'effet se voit sur la figure n'en a pas besoin.
- Un point qu'on fait glisser se saisit par `glisse()` (souris et tactile) ; le
  curseur dit ce qui se saisit (`.saisissable`) ou se clique (`.cliquable`).
- Un point qui se colle à un lieu remarquable (axe, parabole Δ = 0, droite
  ap = 1) prend une valeur **exacte**, calculée par une fonction pure et
  testée : Δ vaut 0, pas 10⁻³.
- Les onglets se parcourent aux flèches ; les bulles s'ouvrent au focus et se
  ferment avec Échap ; un bouton qui ne porte qu'un symbole a un `aria-label`.
- Le zoom d'une courbe temporelle : molette autour du curseur, glisser pour
  déplacer, boutons +, −, ↺ ; le temps reste ≥ 0.
- **Remettre aux valeurs de départ s'écrit « ↺ »** (`bouton-lien`), avec un
  `aria-label` et un `title` qui disent ce qui est remis : « Valeurs par
  défaut », « Zoom initial ». Si l'action efface aussi la mémoire du navigateur
  ([WEB13](#web13--deux-mémoires--lurl-pour-lexpérience-le-navigateur-pour-le-confort)),
  elle demande confirmation. « ↺ » est réservé à ce sens : relancer une
  simulation depuis sa condition initiale garde un libellé en toutes lettres
  (« Réinitialiser »), pour ne pas mettre deux « ↺ » de sens différent sur une
  page.

Vérifié par : tests du socle (`champ`, `ajoutePas`, onglets, infobulles) ; tests
du cours pour l'aimantation.

## WEB15 — Une animation sobre, et pas de calcul qui fige le geste

- On n'anime que ce qui bouge ; une figure immobile n'est pas redessinée à
  chaque image. `prefers-reduced-motion` est respecté : rien ne bouge
  (`outils.mouvementReduit`).
- Une grandeur qui parcourt une suite ou le temps peut s'animer en boucle, avec
  une courte pause, par une case « animer » ; toucher au curseur l'arrête.
- Ce qui se recalcule lentement ne s'efface jamais : l'ancien résultat reste
  affiché pendant le calcul, le nouveau apparaît en fondu (350 ms, sans fondu
  si le mouvement est réduit), et un calcul relancé abandonne le précédent.
- Ce qui ne dépend que des paramètres est gardé en mémoire, car le dessin peut
  être refait à chaque image.

Vérifié par : relecture ; la fumée du socle (`scripts/fumee.py`) pour les
erreurs.

## WEB16 — Le catalogue suit l'ordre du poly

Les thèmes et leurs pages suivent l'ordre du poly, puis l'ordre de difficulté :
on ne présente pas un objet avant la section qui le définit, ni un exercice qui
suppose une page encore à venir. Une page qui couvre deux sections se place à
la première. Cet ordre est l'ordre de lecture (« Page suivante → »).

Vérifié par : relecture ; `verifieCours` (identifiants uniques, parcours bien
formé).

## WEB17 — Un essai décrit exactement ce qu'on voit

- Un essai fixe **tous** les paramètres dont dépend son texte, pour que le texte
  reste vrai même si le lecteur a déplacé un curseur ; s'il le faut, il fixe
  aussi des préférences (`preferences` de l'essai).
- Le point de départ reste visible dans le cadre de la figure.
- Un essai qui demande de se déplacer part d'un **cas voisin** et laisse faire
  le déplacement (« Depuis n = 5, augmentez n »).
- Toute affirmation d'un essai (nature, valeur, verdict) est **vérifiée par
  calcul** avant d'être écrite, par un test du cours.

Vérifié par : `verifieCours` (réglages et préférences d'un essai connus, du bon
type) ; tests du cours pour les affirmations.

## WEB18 — Le code du cours : testé sous Node, sans effet à l'import

- Les fonctions mathématiques d'une page sont exportées par son module et
  testées sous Node contre le poly ou le TD (valeurs, ensembles, aimantation).
- Un module d'illustration ne touche pas au DOM quand on l'importe : il se
  charge sous Node, sans navigateur.
- Le code est en français (noms et commentaires), commenté au niveau du fichier
  voisin.

Vérifié par : `verifieCours` (charge chaque module sous Node) ; tests du cours.

## WEB19 — Une brique du socle plutôt qu'une copie

Ce qui sert à deux cours est une brique du socle ; un cours ne la recopie pas,
il l'importe. Une brique propre à un cours reste dans le cours, dans un fichier
à lui (`js/illustrations/<nom>.js`). Une amélioration d'une brique du socle se
fait dans le socle, par une PR, et le cours monte de version.

Vérifié par : relecture ; `scripts/verifie-cours.py` du socle (les tests de
chaque cours tournent avec le code du socle).

## WEB20 — Vérifier avant de livrer

1. `npm test` dans `site/` (tests du cours et du socle) et `git diff --check`.
2. Ouvrir la page à 1504 × 856 : rien ne défile dans les boîtes, la page ne
   dépasse pas la fenêtre, aucune erreur JavaScript.
3. Ouvrir la page à 1504 × 600 (fenêtre basse), à 1200 × 800 (seule la zone de
   l'illustration défile, pas de double barre) et à 390 × 844 (une colonne qui
   défile).
4. Recharger après un réglage : le réglage revient. Mettre une valeur absurde
   dans l'URL : la page reste cohérente.
5. Cliquer chaque « Essayer » : la figure montre ce que l'essai annonce.
6. Vérifier le thème sombre si l'on a touché aux couleurs.

Vérifié par : la CI du cours (`npm test`, workflow Pages du socle) pour le
point 1 ; la relecture pour les autres. La fumée du socle (`scripts/fumee.py`)
automatise la recherche d'erreurs JavaScript sur une liste de pages.
