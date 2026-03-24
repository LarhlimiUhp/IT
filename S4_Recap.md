

```markdown
# Support Complet de Cours — Chapitre 4 : Les Classes P et NP
## Informatique Théorique — Complexité Computationnelle

---

# PARTIE I — Cours Théorique

---

## Table des matières

1. [Rappels : Machine de Turing](#1-rappels--machine-de-turing)
2. [Temps de calcul et complexité](#2-temps-de-calcul-et-complexité)
3. [La classe P](#3-la-classe-p)
4. [La classe NP](#4-la-classe-np)
5. [P = NP ?](#5-p--np-)
6. [Réductions polynomiales](#6-réductions-polynomiales)
7. [NP-complétude](#7-np-complétude)
8. [Le théorème de Cook-Levin](#8-le-théorème-de-cook-levin)
9. [Problèmes NP-complets classiques](#9-problèmes-np-complets-classiques)
10. [Résumé et carte des réductions](#10-résumé-et-carte-des-réductions)

---

## 1. Rappels : Machine de Turing

### Définition 5.1 — Machine de Turing (MT)

Une machine de Turing est un **6-tuplet** $(Q, \Sigma, \Gamma, \delta, q_0, F)$ où :

| Composante | Description |
|---|---|
| $Q$ | Ensemble fini d'**états** |
| $\Sigma$ | **Alphabet** d'entrée |
| $\Gamma$ | **Alphabet de ruban**, avec $\sqcup \in \Gamma$ (symbole blanc) et $\Sigma \subseteq \Gamma$ |
| $\delta$ | **Fonction de transition** : $Q \times \Gamma \to Q \times \Gamma \times \{\text{GAUCHE}, \text{DROITE}\}$ |
| $q_0$ | **État initial** |
| $F = \{q_A, q_R\}$ | **États finaux** (acceptant et rejetant) |

### Comportement de la MT sur une entrée $w \in \Sigma^*$

- **Accepte** $w$ : si $M$ arrive à l'état $q_A$
- **Rejette** $w$ : si $M$ arrive à l'état $q_R$
- **Boucle** sur $w$ : si $M$ ne s'arrête jamais

### Langage reconnu

$$L(M) = \{w \in \Sigma^* \mid M \text{ accepte } w\}$$

> **Point clé** : On s'intéresse aux MT qui **s'arrêtent sur toutes les entrées** (décideurs).

---

## 2. Temps de calcul et complexité

### 2.1. Temps de calcul d'une MT sur un mot

Soit $M$ une MT qui s'arrête sur toutes les entrées.

$$T(M, w) = \text{nombre d'étapes de calcul de } M \text{ sur entrée } w \text{ avant l'arrêt}$$

Chaque **étape** correspond à une application de la fonction de transition $\delta$.

### 2.2. Complexité en temps (pire cas)

**Définition 5.2** — Le **temps de calcul** de $M$ est la fonction :

$$f : \mathbb{N} \to \mathbb{N}, \quad n \mapsto \max_{|w|=n} T(M, w)$$

On dit que $M$ **fonctionne en temps $f(n)$** ou que sa **complexité en temps** est $f(n)$.

> **Intuition** : On mesure le temps en fonction de la **longueur de l'entrée**, en prenant le **pire cas**.

### 2.3. Classe TIME

**Définition 5.3** :

$$\text{TIME}(t(n)) = \{L \mid L \text{ est décidé par une MT en temps } O(t(n))\}$$

### 2.4. Remarques sur la robustesse

- La définition dépend de l'alphabet et du modèle de MT choisi.
- Une MT avec un grand alphabet peut être simulée en **temps linéaire** par une MT sur un autre alphabet.
- On définit donc la complexité **à une constante près**.
- Différents modèles raisonnables (MT multi-rubans, multi-têtes, etc.) ne sont pas toujours équivalents à une constante près, mais peuvent se simuler en **temps polynomial**.

---

## 3. La classe P

### 3.1. Définition

**Définition 5.4** — La classe **P** :

$$\boxed{\mathsf{P} = \bigcup_{k \geq 0} \text{TIME}(n^k)}$$

P est la classe des langages décidables en **temps polynomial** par une MT déterministe.

### 3.2. Propriétés de P

**Remarque 5.5** : Si $p(n)$ et $q(n)$ sont des polynômes, alors :
- $p(n) + q(n)$ est un polynôme
- $p(n) \cdot q(n)$ est un polynôme
- $p(q(n))$ est un polynôme

→ La classe P est **fermée par composition**.

**Remarque 5.6** — Robustesse :
- P est **robuste** par rapport au modèle de calcul : MT à 1 ruban, $k$ rubans, $k$ têtes, RAM, etc. définissent la même classe.
- P correspond aux langages **décidables en pratique en un temps raisonnable**.

### 3.3. Exemples de problèmes dans P

| Problème | Description |
|---|---|
| PRIMALITÉ | Décider si $n$ est un nombre premier |
| ADDITION MATRICIELLE | Décider si $A + B = C$ pour des matrices d'entiers |
| MULTIPLICATION MATRICIELLE | Décider si $A \times B = C$ pour des matrices d'entiers |
| PLUS COURT CHEMIN | Existence d'un chemin de coût $\leq c$ entre $s$ et $t$ dans $G$ |
| TRI | Décider si une liste est en ordre lexicographique |
| 2-COLORATION | Décider si un graphe est coloriable avec 2 couleurs |
| PROGRAMMATION LINÉAIRE | Décider si un PL admet une solution |
| CIRCUIT-EVAL | Évaluer un circuit booléen sur un input donné |

### 3.4. Un problème difficile : 3-COL

**Définition 5.8** : Un graphe $G$ est **$k$-coloriable** s'il est possible d'assigner à chaque sommet une couleur parmi $k$ couleurs, sans que deux sommets adjacents aient la même couleur.

$$3\text{-}COL = \{\langle G \rangle \mid G \text{ est un graphe 3-coloriable}\}$$

**Question** : A-t-on $3\text{-}COL \in \mathsf{P}$ ?

**Analyse de la recherche exhaustive** :
- Nombre de coloriages possibles : $3^s$ (où $s$ = nombre de sommets)
- Taille de l'encodage : $n = |\langle G \rangle| \in O(s^2 \log s)$
- Temps : $\Omega\left(3^{\sqrt{\frac{n}{\log n}}}\right)$, qui n'est borné par **aucun polynôme**

→ La stratégie exhaustive n'est **pas polynomiale**. Existe-t-il une meilleure stratégie ?

---

## 4. La classe NP

### 4.1. Vérificateur polynomial

**Définition 5.9** — Un **vérificateur polynomial** pour un langage $L$ est une MT $V$ telle que pour tout $w \in \Sigma^*$ :
- Si $w \in L$ : il existe un mot $c$ (le **certificat**) tel que $\langle w, c \rangle \in L(V)$
- Si $w \notin L$ : pour tout $c$, on a $\langle w, c \rangle \notin L(V)$
- Le temps de calcul de $V$ sur $\langle w, c \rangle$ est **polynomial en $|w|$**

> Le mot $c$ est appelé **certificat**, **preuve** ou **témoin** de l'appartenance de $w$ à $L$.

### 4.2. Définition de NP

**Définition 5.10** :

$$\boxed{\mathsf{NP} = \text{l'ensemble des langages qui possèdent un vérificateur polynomial}}$$

> **Intuition** : NP = langages pour lesquels on peut **vérifier efficacement** une solution proposée, même si on ne sait pas la **trouver** efficacement.

### 4.3. Exemples de problèmes dans NP

#### COMPOSÉ

$$COMPOSÉ = \{\langle n \rangle \mid n \in \mathbb{N} \text{ et } n \text{ est composé}\}$$

**Théorème 5.12** : $COMPOSÉ \in \mathsf{NP}$.

*Certificat* : une paire $(x, y)$ avec $1 < x, y < n$ et $xy = n$.
*Vérification* : multiplier et comparer → temps polynomial.

**Théorème 5.13 (Agrawal-Kayal-Saxena, 2004)** : $COMPOSÉ \in \mathsf{P}$.

#### 3-COL

**Théorème 5.14** : $3\text{-}COL \in \mathsf{NP}$.

*Certificat* : un 3-coloriage (assignation d'une couleur à chaque sommet).
*Vérification* : pour chaque arête, vérifier que les deux extrémités ont des couleurs différentes → $O(n^2)$ vérifications.

#### CLIQUE

$$CLIQUE = \{\langle G, k \rangle \mid G \text{ contient un sous-graphe complet de taille } k\}$$

**Théorème 5.21** : $CLIQUE \in \mathsf{NP}$.

#### SUBSET-SUM

$$SUBSET\text{-}SUM = \left\{\langle \{x_1, \ldots, x_m\}, t \rangle \mid \exists \{y_1, \ldots, y_l\} \subseteq \{x_1, \ldots, x_m\} : \sum_{i=1}^l y_i = t\right\}$$

**Théorème 5.23** : $SUBSET\text{-}SUM \in \mathsf{NP}$.

#### HAMPATH

$$HAMPATH = \{\langle G, s, t \rangle \mid \text{un chemin hamiltonien existe de } s \text{ à } t \text{ dans } G\}$$

Un **chemin hamiltonien** entre $s$ et $t$ est un chemin orienté passant exactement une fois par chaque sommet.

**Théorème 5.17** : $HAMPATH \in \mathsf{NP}$.
*Certificat* : le chemin hamiltonien lui-même.

---

## 5. P = NP ?

### 5.1. Résumé des deux classes

| Classe | Caractérisation |
|---|---|
| **P** | Langages **décidables** efficacement (en temps polynomial) |
| **NP** | Langages **vérifiables** efficacement (en temps polynomial) |

### 5.2. Relations connues

**Théorème 5.19** :

$$\boxed{\mathsf{P} \subseteq \mathsf{NP} \subseteq \mathsf{EXPTIME}}$$

*Preuve de* $\mathsf{P} \subseteq \mathsf{NP}$ : Si $L \in \mathsf{P}$, le décideur polynomial sert de vérificateur (on ignore le certificat).

*Preuve de* $\mathsf{NP} \subseteq \mathsf{EXPTIME}$ :
Soit $L \in \mathsf{NP}$ avec vérificateur $V$ de temps polynomial $p(n)$.
Un décideur $D$ teste **tous les certificats** de longueur $\leq p(n)$ :

$$t(n) \approx d^{p(n)} \cdot p(n) \approx 2^{p(n) \log d + \log p(n)} \in O(2^{n^k})$$

### 5.3. La question à un million de dollars

$$\boxed{\mathsf{P} = \mathsf{NP} \text{ ?}}$$

- Le **Clay Mathematical Institute** offre **1 000 000 \$** pour la résolution.
- C'est l'un des 7 problèmes du millénaire.
- La réponse est **toujours inconnue** — c'est le problème central de la théorie de la complexité.
- La majorité des chercheurs conjecturent $\mathsf{P} \neq \mathsf{NP}$.

---

## 6. Réductions polynomiales

### 6.1. Définition

**Définition 5.24** — On dit que $L_1$ **se réduit polynomialement** à $L_2$, noté :

$$L_1 \leq_{\mathsf{P}} L_2$$

s'il existe une fonction $f : \Sigma^* \to \Sigma^*$ **calculable en temps polynomial** telle que :

$$\forall x : \quad x \in L_1 \iff f(x) \in L_2$$

> **Intuition** : Résoudre $L_1$ est **au plus aussi difficile** que résoudre $L_2$ (à un facteur polynomial près).

### 6.2. Propriétés fondamentales

**Théorème 5.25** — *Transitivité vers P* :

$$\text{Si } L_1 \leq_{\mathsf{P}} L_2 \text{ et } L_2 \in \mathsf{P}, \text{ alors } L_1 \in \mathsf{P}.$$

*Preuve* : Construire $M_1$ qui calcule $w' = f(w)$ puis simule $M_2$ sur $w'$. Temps : $O(q(p(n)))$ = polynomial.

**Théorème 5.26** — *Transitivité vers NP* :

$$\text{Si } L_1 \leq_{\mathsf{P}} L_2 \text{ et } L_2 \in \mathsf{NP}, \text{ alors } L_1 \in \mathsf{NP}.$$

*Preuve* : On construit un vérificateur $V_1$ pour $L_1$ à partir du vérificateur $V_2$ pour $L_2$ et de la réduction $f$.

---

## 7. NP-complétude

### 7.1. Définitions

**Définition 5.27** — **NP-difficile** :

Un langage $L$ est **NP-difficile** si pour tout $L' \in \mathsf{NP}$, on a $L' \leq_{\mathsf{P}} L$.

**Définition 5.28** — **NP-complet** :

Un langage $L$ est **NP-complet** si :
1. $L$ est **NP-difficile**
2. $L \in \mathsf{NP}$

La classe des langages NP-complets est notée **NPC**.

### 7.2. Interprétation intuitive

**Remarque 5.29** : Si $L$ est NP-complet, alors :
- $L$ est **au moins aussi difficile** que n'importe quel problème de NP
- $L$ n'est **pas trop difficile** car il est lui-même dans NP

### 7.3. Le cœur de la NP-complétude

**Théorème 5.30** — *Théorème fondamental* :

$$\boxed{\text{Si } L \text{ est NP-complet et } L \in \mathsf{P}, \text{ alors } \mathsf{P} = \mathsf{NP}.}$$

*Preuve* :
- On sait $\mathsf{P} \subseteq \mathsf{NP}$ (trivial).
- Soit $A \in \mathsf{NP}$. Comme $L$ est NP-difficile : $A \leq_{\mathsf{P}} L$.
- Or $L \in \mathsf{P}$, donc $A \in \mathsf{P}$.
- D'où $\mathsf{NP} \subseteq \mathsf{P}$, et finalement $\mathsf{P} = \mathsf{NP}$.

### 7.4. Méthode pour prouver la NP-complétude

**Théorème 5.41** : Soit $L_1$ un langage **NP-difficile**. Si $L_2 \in \mathsf{NP}$ et $L_1 \leq_{\mathsf{P}} L_2$, alors $L_2$ est **NP-complet**.

**Méthode en deux étapes** :
1. Montrer que $L_2 \in \mathsf{NP}$ (exhiber un certificat et un vérificateur polynomial)
2. Montrer qu'un problème NP-complet connu $L_1$ se réduit à $L_2$ : $L_1 \leq_{\mathsf{P}} L_2$

---

## 8. Le théorème de Cook-Levin

### 8.1. Circuits booléens

**Définition 5.31** : Les opérateurs $\neg$, $\wedge$, $\vee$ dans un circuit booléen sont appelés des **portes**.

**Définition 5.32** : La **taille** d'un circuit = nombre de portes.

**Définition 5.33** : Un circuit est **satisfaisable** s'il existe une affectation de ses variables le rendant vrai.

### 8.2. SAT-C (Circuit-SAT)

**Définition 5.36** :

$$SAT\text{-}C = \{\langle C \rangle \mid C \text{ est un circuit booléen satisfaisable}\}$$

### 8.3. Le théorème

**Théorème 5.37 (Cook-Levin)** :

$$\boxed{SAT\text{-}C \text{ est NP-complet.}}$$

#### Idée de la preuve

**Étape 1** : $SAT\text{-}C \in \mathsf{NP}$ (trivial : le certificat est une affectation satisfaisante).

**Étape 2** : Pour tout $L \in \mathsf{NP}$, on montre $L \leq_{\mathsf{P}} SAT\text{-}C$.

**Construction clé** (Théorème 5.38) : Pour toute MT $M$ fonctionnant en temps polynomial $p(n)$, on peut construire une famille de circuits $\{C_n\}$ de taille polynomiale simulant $M$.

**Variables du circuit** (pour la trace de calcul de $M$) :

| Variable | Signification |
|---|---|
| $ZÉRO(i,j)$ | Case $j$ au temps $i$ contient **0** |
| $UN(i,j)$ | Case $j$ au temps $i$ contient **1** |
| $BLANC(i,j)$ | Case $j$ au temps $i$ contient **⊔** |
| $ÉTAT(q,i,j)$ | L'état de $M$ est $q$ au temps $i$, tête en position $j$ |

Nombre de variables : $3p(n)^2 + p(n)^2 \cdot l$ (où $l = |Q|$).

Chaque variable au temps $i > 1$ est une **fonction booléenne** d'au plus $3l + 9$ variables du temps $i-1$, déterminée par $\delta$.

Sortie du circuit :

$$ACCEPTE = \bigvee_{\substack{i \geq 1 \\ j \geq 1}} ÉTAT(q_a, i, j)$$

### 8.4. Chaîne de réductions à partir de SAT-C

```
SAT-C  →  SAT  →  SAT-FNC  →  3-SAT  →  CLIQUE
                                    ↓           ↓
                                  3-COL    COUVERTURE
                                    ↓
                                 HORAIRE

                              3-SAT  →  PROGR-ENT

                              CLIQUE ↔ ANTICLIQUE ↔ COUVERTURE
```

---

## 9. Problèmes NP-complets classiques

### 9.1. SAT (Satisfaisabilité d'expressions booléennes)

**Définition 5.42** : Une **expression booléenne** est un circuit booléen ayant une structure d'**arbre**.

**Définition 5.44** :

$$SAT = \{\langle E \rangle \mid E \text{ est une expression booléenne satisfaisable}\}$$

**Théorème 5.45** : $SAT \in \mathsf{NP}$ (certificat = affectation).

**Théorème 5.46** : $SAT$ est **NP-complet**.

*Preuve* : On montre $SAT\text{-}C \leq_{\mathsf{P}} SAT$ en transformant chaque circuit en expression via :
$$E'_i = (x_i \wedge E_i) \vee (\neg x_i \wedge \neg E_i), \quad E = x_l \wedge E'_1 \wedge E'_2 \wedge \ldots \wedge E'_l$$

### 9.2. SAT-FNC (Satisfaisabilité en Forme Normale Conjonctive)

**Définition 5.47** :
- **Terme** (ou littéral) : variable $x$ ou sa négation $\neg x$
- **Clause** : disjonction (somme) de termes
- **FNC** : conjonction (produit) de clauses

**Exemple** : $(x_1 \vee \neg x_2 \vee x_3) \wedge (x_3 \vee x_4) \wedge (\neg x_1 \vee \neg x_4)$

**Théorème 5.51** : $SAT\text{-}FNC$ est **NP-complet**.

*Preuve* : $SAT \leq_{\mathsf{P}} SAT\text{-}FNC$ par :
1. Représenter $E$ comme arbre syntaxique
2. Descendre les négations aux feuilles (lois de De Morgan)
3. Transformer chaque $E_1 \vee E_2$ en $E_1^y \wedge E_2^{\neg y}$ (introduction de nouvelle variable $y$)

### 9.3. 3-SAT

**Définition 5.53** : Expression en **3-FNC** = FNC dont chaque clause contient **exactement 3 littéraux** de variables distinctes.

**Définition 5.55** :

$$3\text{-}SAT = \{\langle \phi \rangle \mid \phi \text{ est en 3-FNC et est satisfaisable}\}$$

**Théorème 5.57** : $3\text{-}SAT$ est **NP-complet**.

*Réduction* $SAT\text{-}FNC \leq_{\mathsf{P}} 3\text{-}SAT$ par transformation de chaque clause :

| Clause originale | Transformation en 3-FNC |
|---|---|
| $(t)$ | $(t \vee y \vee z) \wedge (t \vee y \vee \neg z) \wedge (t \vee \neg y \vee z) \wedge (t \vee \neg y \vee \neg z)$ |
| $(t_1 \vee t_2)$ | $(t_1 \vee t_2 \vee y) \wedge (t_1 \vee t_2 \vee \neg y)$ |
| $(t_1 \vee t_2 \vee t_3)$ | inchangée |
| $(t_1 \vee \ldots \vee t_k)$, $k > 3$ | $(t_1 \vee t_2 \vee y_1) \wedge (\neg y_1 \vee t_3 \vee y_2) \wedge \ldots \wedge (\neg y_{k-3} \vee t_{k-1} \vee t_k)$ |

### 9.4. CLIQUE

**Théorème 5.59** : $CLIQUE$ est **NP-complet**.

*Réduction* $3\text{-}SAT \leq_{\mathsf{P}} CLIQUE$ :

Pour $\phi = (t_{1,1} \vee t_{1,2} \vee t_{1,3}) \wedge \ldots \wedge (t_{k,1} \vee t_{k,2} \vee t_{k,3})$ :

- **Sommets** : $3k$ sommets correspondant aux termes $t_{i,j}$
- **Arêtes** : entre $t_{i_1,j_1}$ et $t_{i_2,j_2}$ ssi $i_1 \neq i_2$ (clauses différentes) **et** $t_{i_1,j_1} \neq \neg t_{i_2,j_2}$ (pas contradictoires)
- $f(\langle \phi \rangle) = \langle G, k \rangle$

**Correction** : $\phi$ satisfaisable $\iff$ $G$ contient une clique de taille $k$.

### 9.5. COUVERTURE (Vertex Cover)

**Définition 5.60** : Une **couverture de sommets** de $G$ est un sous-ensemble $S$ des sommets tel que chaque arête de $G$ a au moins une extrémité dans $S$.

$$COUVERTURE = \{\langle G, s \rangle \mid G \text{ possède une couverture de taille } s\}$$

**Théorème 5.64** : $COUVERTURE$ est **NP-complet**.

*Aperçu* : $CLIQUE \leq_{\mathsf{P}} COUVERTURE$ via le complément du graphe :
$$G \text{ a une clique de taille } k \iff \bar{G} \text{ a une couverture de taille } n - k$$

**Remarque 5.65** : On a aussi $COUVERTURE \leq_{\mathsf{P}} CLIQUE$.

### 9.6. ANTICLIQUE (Ensemble indépendant)

**Définition 5.66** : Une **anticlique** (ou **ensemble indépendant**) est un sous-ensemble de sommets sans arête entre eux.

$$ANTICLIQUE = \{\langle G, k \rangle \mid G \text{ contient une anticlique de taille } k\}$$

**Théorème 5.68** : $ANTICLIQUE$ est **NP-complet**.

*Preuve* : Clique de taille $k$ dans $G$ $\iff$ anticlique de taille $k$ dans $\bar{G}$.

### 9.7. PROGR-ENT (Programmation en nombres entiers)

**Définition 5.69** : $PROGR\text{-}ENT$ contient les paires $\langle A, b \rangle$ telles que le système $Ax \leq b$ admet une solution **entière**.

**Théorème 5.70** : $PROGR\text{-}ENT$ est **NP-complet**.

*Réduction* : $3\text{-}SAT \leq_{\mathsf{P}} PROGR\text{-}ENT$ via la correspondance :

| Clause | Inéquation (avec $0 \leq x_i \leq 1$) |
|---|---|
| $x_{i_1} \vee x_{i_2} \vee x_{i_3}$ | $x_{i_1} + x_{i_2} + x_{i_3} \geq 1$ |
| $x_{i_1} \vee x_{i_2} \vee \neg x_{i_3}$ | $x_{i_1} + x_{i_2} + (1-x_{i_3}) \geq 1$ |
| $\neg x_{i_1} \vee \neg x_{i_2} \vee \neg x_{i_3}$ | $(1-x_{i_1}) + (1-x_{i_2}) + (1-x_{i_3}) \geq 1$ |

### 9.8. 3-COL

**Définition 5.71** : $3\text{-}COL = \{\langle G \rangle \mid G \text{ est 3-coloriable}\}$

**Théorème 5.72** : $3\text{-}COL$ est **NP-complet**.

*Réduction* : $3\text{-}SAT \leq_{\mathsf{P}} 3\text{-}COL$ via une construction de graphe comportant :
- **5 sommets par clause** (formant un pentagone)
- **2 sommets par variable** ($x_j$ et $\neg x_j$)
- **2 sommets accessoires** ($a$ et $b$)
- Arêtes reliant ces structures de façon à encoder la satisfaisabilité

### 9.9. COL (k-coloration générale)

**Définition 5.73** : $COL = \{\langle G, k \rangle \mid G \text{ est } k\text{-coloriable}\}$

**Théorème 5.74** : $COL$ est **NP-complet** ($3\text{-}COL \leq_{\mathsf{P}} COL$ trivialement).

**Remarque 5.75** : Pour $k > 2$, les langages $k$-$COL$ sont NP-complets, mais les langages $k$-$COUVERTURE$, $k$-$CLIQUE$ et $k$-$ANTICLIQUE$ **ne le sont pas** (ils sont dans P pour $k$ fixé).

### 9.10. HORAIRE

**Définition 5.76** : Encodage d'un problème d'emploi du temps avec activités, individus, contraintes et $s$ plages horaires.

**Théorème 5.77** : $HORAIRE$ est **NP-complet** ($3\text{-}COL \leq_{\mathsf{P}} HORAIRE$).

---

## 10. Résumé et carte des réductions

### 10.1. Hiérarchie des classes

```
╔══════════════════════════════════════════════════════╗
║                     EXPTIME                          ║
║  ┌─────────────────────────────────────────────┐     ║
║  │                    NP                        │     ║
║  │  ┌───────────────────────────────────┐       │     ║
║  │  │              NPC                   │       │     ║
║  │  │  SAT-C, SAT, SAT-FNC, 3-SAT,     │       │     ║
║  │  │  CLIQUE, COUVERTURE, ANTICLIQUE,  │       │     ║
║  │  │  3-COL, COL, HAMPATH,            │       │     ║
║  │  │  SUBSET-SUM, PROGR-ENT, HORAIRE  │       │     ║
║  │  └───────────────────────────────────┘       │     ║
║  │  ┌──────────────────┐                        │     ║
║  │  │        P          │                        │     ║
║  │  │  Primalité, Tri,  │                        │     ║
║  │  │  2-COL, PL, ...   │                        │     ║
║  │  └──────────────────┘                        │     ║
║  └─────────────────────────────────────────────┘     ║
╚══════════════════════════════════════════════════════╝
```

### 10.2. Chaîne de réductions polynomiales

```
         SAT-C (Cook-Levin)
           │
           ▼
          SAT
           │
           ▼
        SAT-FNC
           │
           ▼
         3-SAT
        ╱  │  ╲
       ╱   │   ╲
      ▼    ▼    ▼
  CLIQUE  3-COL  PROGR-ENT
     │      │
     ▼      ▼
COUVERTURE  HORAIRE
     │
     ▼
 ANTICLIQUE
```

### 10.3. Tableau récapitulatif

| Concept | Définition courte |
|---|---|
| **P** | Décidable en temps polynomial |
| **NP** | Vérifiable en temps polynomial |
| **NP-difficile** | Tout problème NP s'y réduit |
| **NP-complet** | NP-difficile + dans NP |
| **Réduction** $L_1 \leq_P L_2$ | $f$ polynomiale : $x \in L_1 \iff f(x) \in L_2$ |
| **Certificat** | Preuve vérifiable en temps polynomial |

### 10.4. Questions clés pour les examens

1. **Montrer qu'un problème est dans NP** : exhiber un certificat et montrer que la vérification est polynomiale.
2. **Montrer qu'un problème est NP-complet** : (i) montrer $\in$ NP, (ii) réduire un problème NP-complet connu vers celui-ci.
3. **Conséquence de P = NP** : Tous les problèmes NP-complets seraient résolubles en temps polynomial.
4. **Conséquence de P ≠ NP** : Aucun problème NP-complet n'est dans P.

---

> **À retenir** : La NP-complétude identifie les problèmes les plus difficiles de NP. Prouver qu'un seul d'entre eux est dans P résoudrait le problème P vs NP — l'un des plus grands défis ouverts des mathématiques et de l'informatique.

---
---

# PARTIE II — Exercices des Démonstrations 8 & 9 (avec corrections)

---

## 11. Exercices sur les langages hors-contexte

### Exercice 1 — Le langage $L = \{ww \mid w \in \Sigma^*\}$ n'est pas hors-contexte

**Énoncé** : Démontrer que $L := \{ww \mid w \in \Sigma^*\}$ n'est pas hors-contexte.

**Solution (par le lemme de pompage pour les langages HC)** :

**Raisonnement par l'absurde** : Supposons $L \in \text{HC}$ avec longueur de pompe $p$.

**Choix du mot** : Soit $W = 0^p 1^p 0^p 1^p$. Clairement $W \in L$ (avec $w = 0^p 1^p$) et $|W| = 4p \geq p$.

Par le lemme de pompage, on peut écrire $W = uvxyz$ avec $|vxy| \leq p$ et $|vy| \geq 1$.

**Analyse par cas** :

| Cas | Position de $vxy$ | Pompage | Contradiction |
|---|---|---|---|
| **Cas 1** | Entièrement dans la **première moitié** ($0^p 1^p$) | $i = 0$ | $W' = uxy = wt$ avec $|w| = |t|$, mais $w$ se termine par des 0 et $t$ par des 1 → $W' \notin L$ |
| **Cas 2** | Entièrement dans la **seconde moitié** ($0^p 1^p$) | $i = 0$ | Le premier « mot » commence par 0, le second par 1 → $W' \notin L$ |
| **Cas 3** | **Au milieu** du mot (chevauche les deux moitiés) | $i = 0$ | $uyz = 0^p 1^i 0^j 1^p$ avec $i < p$ ou $j < p$. Donc soit $w$ a plus de 0 que $t$, soit $t$ a plus de 1 que $w$ → $W' \notin L$ |

**Conclusion** : Dans tous les cas, on obtient une contradiction. Donc $L \notin \text{HC}$. $\square$

> **Méthode clé** : Le lemme de pompage pour les langages hors-contexte affirme que pour tout $L \in \text{HC}$, il existe $p$ tel que tout mot $w \in L$ avec $|w| \geq p$ peut s'écrire $w = uvxyz$ avec $|vxy| \leq p$, $|vy| \geq 1$, et $\forall i \geq 0 : uv^i xy^i z \in L$.

---

### Exercice 2 — Grammaire généralisée pour $S = \{0^{n^2} \mid n \in \mathbb{N}\}$

**Énoncé** : Trouver une grammaire généralisée pour $S := \{0^{n^2} \mid n \in \mathbb{N}\}$.

**Solution** :

**Idée** : On génère d'abord un nombre arbitraire de symboles $P$ (disons $n$ occurrences). Ensuite, on « met au carré » ce nombre en barrant un $P$ à la fois et en ajoutant un $a$ pour chaque $P$ (barré ou non) à chaque passe. On s'arrête lorsque tous les $P$ sont barrés.

**Variables** : $V = \{S, E, C, P, X, T_1, T_2, R, T_3\}$

| Variable | Rôle |
|---|---|
| $S$ | Axiome |
| $E$ | Marque le début et la fin du mot |
| $C$ | Curseur de génération |
| $P$ | Variable à « mettre au carré » |
| $X$ | Un $P$ barré (déjà compté) |
| $T_1$ | Passe en cours, aucun $P$ encore barré |
| $T_2$ | Un $P$ a été barré dans cette passe |
| $R$ | Retour au début pour la prochaine passe |
| $T_3$ | Tous les $P$ sont barrés |

**Règles de production** :

$$\begin{aligned}
&(1) \quad S \to ECE \\
&(2) \quad C \to CP \mid T_1 \\
&(3) \quad T_1 P \to aXT_2 \\
&(4) \quad T_1 X \to aXT_1 \\
&(5) \quad T_1 a \to aT_1 \\
&(6) \quad T_2 a \to aT_2 \\
&(7) \quad T_2 P \to aPT_2 \\
&(8) \quad PT_2 E \to RPE \\
&(9) \quad aR \to Ra \\
&(10) \quad XR \to RX \\
&(11) \quad PR \to RP \\
&(12) \quad ER \to ET_1 \\
&(13) \quad XT_2 E \to T_3 \\
&(14) \quad aT_3 \to T_3 a \\
&(15) \quad XT_3 \to T_3 \\
&(16) \quad ET_3 a \to a
\end{aligned}$$

> **Remarque** : Certaines règles sont **contractantes** (le membre droit est plus court que le gauche), donc il s'agit d'une **grammaire contextuelle généralisée** (non restreinte), et non d'une grammaire simplement contextuelle.

**Exemple de dérivation** : Production de $a^9 = 0^{3^2}$ :

$$S \xrightarrow{1} ECE \xrightarrow{2} ECPE \xrightarrow{2} ECPPE \xrightarrow{2} ECPPPE \xrightarrow{2} ET_1PPPE$$

$$\xrightarrow{3} EaXT_2PPE \xrightarrow{7} EaXaPT_2PE \xrightarrow{7} EaXaPaPT_2E$$

$$\xrightarrow{8} EaXaPaRPE \xrightarrow{9,10,11,12} ET_1aXaPaPE$$

$$\xrightarrow{3,4,5,6,7} EaaXaaXaaPT_2E \xrightarrow{8} EaaXaaXaaRPE$$

$$\xrightarrow{9,10,11} ERaaXaaXaaPE \xrightarrow{12} ET_1aaXaaXaaPE$$

$$\xrightarrow{3,4,5} EaaaXaaaXaaaXT_2E \xrightarrow{13} EaaaXaaaXaaaT_3$$

$$\xrightarrow{14,15,16} aaaaaaaaa = a^9$$

---

### Exercice 3 — Le complément de $L = \{a^n b^n c^n \mid n \in \mathbb{N}\}$ est hors-contexte

**Énoncé** : Montrer que $\bar{L}$ est hors-contexte.

**Solution** :

On décompose le complément :

$$\bar{L} = L_{\text{alt}} \cup L_{\neq}$$

**Partie 1** — $L_{\text{alt}}$ : mots qui ne respectent pas l'ordre $a^* b^* c^*$

$$L_{\text{alt}} = \{w \in \{a, b, c\}^* \mid w \text{ contient un } a \text{ après un } b \text{ ou un } c, \text{ ou un } b \text{ après un } c\}$$

Ce langage est **régulier** (reconnaissable par un AFN simple qui détecte les alternances interdites).

**Partie 2** — $L_{\neq}$ : mots de la forme $a^n b^m c^l$ avec $n \neq m$ ou $m \neq l$

$$L_{\neq} = L_1 \cup L_2 \quad \text{où} \quad L_1 = \{a^n b^m c^l \mid n \neq m\}, \quad L_2 = \{a^n b^m c^l \mid m \neq l\}$$

**Grammaire hors-contexte pour $L_1$** ($n \neq m$) :

$$\begin{aligned}
S &\to S_1 \mid S_2 \\
S_1 &\to aX_1 C \quad &\text{(plus de } a \text{ que de } b\text{)} \\
C &\to CC \mid c \mid \varepsilon \\
X_1 &\to aX_1 \mid aX_1 b \mid \varepsilon \\
S_2 &\to X_2 bC \quad &\text{(plus de } b \text{ que de } a\text{)} \\
X_2 &\to X_2 b \mid aX_2 b \mid \varepsilon
\end{aligned}$$

On construit de manière analogue une grammaire pour $L_2$.

**Conclusion** : Puisque $L_{\text{alt}}$ est régulier (donc HC) et $L_{\neq}$ est HC, et que les langages HC sont **fermés par union**, on a $\bar{L} \in \text{HC}$. $\square$

> **Point important** : Cet exercice montre que les langages hors-contexte ne sont **pas fermés par complément**. En effet, $L = \{a^n b^n c^n\} \notin \text{HC}$ mais $\bar{L} \in \text{HC}$.

---

### Exercice bonus — Lemme de pompage appliqué à $L = \{a^n b^n c^m \mid n, m \in \mathbb{N}, m < n\}$

**Énoncé** (Quiz 6) : Montrer que $L \notin \text{HC}$.

**Solution** :

**Mot choisi** : $w = a^{p+1} b^{p+1} c^p$, avec $p$ la longueur de pompage. On écrit $w = uvxyz$.

| Cas | Description | Pompage | Résultat |
|---|---|---|---|
| **Cas 1** | $v$ ou $y$ contient **deux types de symboles** | $i = 2$ | Alternance des symboles → mot hors de $a^* b^* c^*$ → $\notin L$ |
| **Cas 2** | $v, y$ ne contiennent **aucun** $c$ (un seul type chacun) | $i = 0$ | $a^l b^r c^p$ avec $l \leq p$ ou $r \leq p$ → condition $m < n$ violée ou $n \neq m'$ |
| **Cas 3** | $v, y$ contiennent au moins un $c$ ; $v$ contient des $b$ | $i = 0$ | Moins de $b$ que de $a$ initialement, et la condition se casse |
| **Cas 4** | $v, y$ ne contiennent que des $c$ | $i = 2$ | $a^{p+1} b^{p+1} c^{p+r}$ avec $r > 0$, donc $p + r \geq p + 1$ → $m \geq n$ → $\notin L$ |

**Conclusion** : $L \notin \text{HC}$. $\square$

---

## 12. Exercices sur l'appartenance à NP

### Exercice 4 — SUBSET-SUM $\in$ NP

**Énoncé** : Soit $L = \{(S, t) \mid S \subseteq \mathbb{N}, \exists\, T \subseteq S \text{ t.q. } \sum_{y \in T} y = t\}$. Montrer que $L \in \text{NP}$.

**Solution** :

**Certificat (témoin)** : Un sous-ensemble $T \subseteq S$ tel que $\sum_{y \in T} y = t$.

**Vérificateur** $V$ sur entrée $(S, t, T)$ :

```
Algorithme V(S, t, T) :
1. Pour chaque y ∈ T :
     Vérifier que y ∈ S     → O(|S|) par élément
2. Calculer s = Σ_{y∈T} y   → O(|T|) additions
3. Si s = t : ACCEPTER
   Sinon : REJETER
```

**Analyse** :
- **Taille du certificat** : $|T| \leq |S|$ → polynomiale en la taille de l'entrée
- **Temps de vérification** : $O(|S| \cdot |T| + |T|) = O(|S|^2)$ → **polynomial**

**Correction** :
- Si $(S, t) \in L$ : il existe $T$ tel que $V$ accepte $(S, t, T)$ ✓
- Si $(S, t) \notin L$ : pour tout $T$, la somme $\neq t$, donc $V$ rejette ✓

**Conclusion** : $L \in \text{NP}$. $\square$

---

### Exercice 5 (Démo 8) — Isomorphisme de graphes $\in$ NP

**Énoncé** : Soit $L = \{(G, H) \mid G \text{ est isomorphe à } H\}$. Montrer que $L \in \text{NP}$.

**Solution** :

Soient $G = (V, E)$ et $H = (V', E')$.

**Certificat** : Un isomorphisme $\sigma : V \to V'$ (une bijection).

**Vérificateur** :

```
Algorithme V(G, H, σ) :
1. Pour chaque paire (u, v) ∈ V × V :
     Vérifier que (u,v) ∈ E  ⟺  (σ(u), σ(v)) ∈ E'
2. Si toutes les vérifications passent : ACCEPTER
   Sinon : REJETER
```

**Analyse** :
- Nombre de paires à vérifier : $\binom{n}{2} \in O(n^2)$
- Chaque vérification : $O(1)$ (avec une matrice d'adjacence)
- **Temps total** : $O(n^2)$ → **polynomial**

**Conclusion** : $L \in \text{NP}$. $\square$

> **Remarque** : La question de savoir si l'isomorphisme de graphes est dans P est un **problème ouvert célèbre**. On sait qu'il est dans NP, mais on ne sait pas s'il est NP-complet (on pense que non).

---

### Exercice 6 (Démo 8) — Plus court chemin pondéré $\in$ NP (et même $\in$ P)

**Énoncé** : Soit $L = \{(G, u, v, t) \mid G = (V, E) \text{ graphe pondéré, } \exists \text{ chemin de poids } \leq t \text{ de } u \text{ à } v\}$. Montrer que $L \in \text{NP}$.

**Solution** :

**Certificat** : La chaîne vide $\varepsilon$ (aucun témoin nécessaire !).

**Vérificateur** $V$ sur entrée $(G, u, v, t, \varepsilon)$ :

```
Algorithme V(G, u, v, t, ε) :
1. Exécuter l'algorithme de Dijkstra sur G entre u et v
2. Soit d le poids du plus court chemin trouvé
3. Si d ≤ t : ACCEPTER
   Sinon : REJETER
```

**Analyse** :
- Dijkstra s'exécute en $O(|V|^2)$ ou $O(|E| + |V| \log |V|)$ → **polynomial**
- Le vérificateur n'a pas besoin de certificat car l'algorithme de Dijkstra résout le problème directement

**Conclusion** : $L \in \text{NP}$. En fait, $L \in \mathsf{P}$ (puisque Dijkstra est polynomial). $\square$

> **Point pédagogique** : Tout langage dans P est aussi dans NP ($\mathsf{P} \subseteq \mathsf{NP}$). Pour les problèmes dans P, le vérificateur peut ignorer le certificat et résoudre le problème directement.

---

### Exercice 5 (Démo 8, version originale) — Logarithme discret $\in$ NP

**Énoncé** : Soit $p$ premier, $(\mathbb{Z}/p\mathbb{Z})^*$ le groupe multiplicatif. On dit que $g \in \mathbb{Z}/p\mathbb{Z}$ est d'**ordre** $q$ si $q | (p-1)$ et $q = \min_{r > 0}\{r \mid g^r = 1\}$.

$$L = \{(p, q, g, h) \mid p \text{ premier}, q | (p-1), g \text{ d'ordre } q, \exists w \text{ t.q. } g^w = h\}$$

Montrer que $L \in \text{NP}$.

**Solution** :

**Certificat** : L'entier $w$ tel que $g^w \equiv h \pmod{p}$.

**Vérificateur** :

```
Algorithme V(p, q, g, h, w) :
1. Vérifier que p est premier (via AKS, polynomial)
2. Vérifier que q | (p-1)
3. Vérifier que g^q ≡ 1 (mod p) (exponentiation modulaire rapide)
4. Vérifier que g^w ≡ h (mod p) (exponentiation modulaire rapide)
5. Si tout est vérifié : ACCEPTER
   Sinon : REJETER
```

**Analyse** :
- $|w| \leq |q| \leq |p|$ → taille polynomiale du certificat
- L'exponentiation modulaire rapide s'effectue en $O(\log w \cdot (\log p)^2)$ → **polynomial**
- Le test de primalité AKS est polynomial

**Conclusion** : $L \in \text{NP}$. $\square$

---

### Exercice 7 (Démo 8) — $L_{\text{fact}}$ et son complément dans NP

**Énoncé** : Soit $L_{\text{fact}} = \{(n, k) \mid \exists d \text{ t.q. } 1 < d \leq k, d | n\}$.

**(a)** Montrer que $L_{\text{fact}} \in \text{NP}$.

**(b)** Montrer que $L_{\text{fact}} \in \text{coNP}$ (i.e., $\bar{L}_{\text{fact}} \in \text{NP}$).

**Solution (a)** — $L_{\text{fact}} \in \text{NP}$ :

**Certificat** : Un entier $d$ avec $1 < d \leq k$ et $d | n$.

**Vérificateur** $V((n, k), d)$ :

```
1. Si d = 1 ou d > k : REJETER
2. Si d | n : ACCEPTER
   Sinon : REJETER
```

Les deux tests se font en temps polynomial. $\square$

**Solution (b)** — $\bar{L}_{\text{fact}} \in \text{NP}$ :

On considère $\bar{L}_{\text{fact}} = \{(n, k) \mid \text{tous les diviseurs de } n \text{ sont } > k\}$.

**Certificat** : La factorisation complète $(p_1, e_1), \ldots, (p_m, e_m)$ telle que $\prod_i p_i^{e_i} = n$.

> Par le **théorème fondamental de l'arithmétique**, cette factorisation est **unique**.

**Vérificateur** $V((n, k), (p_1, e_1), \ldots, (p_m, e_m))$ :

```
1. Pour i = 1, ..., m :
     Si p_i ≤ k : REJETER
2. Pour i = 1, ..., m :
     Si p_i est composé (test AKS) : REJETER
3. Si ∏ p_i^{e_i} = n : ACCEPTER
   Sinon : REJETER
```

Tous ces tests se font en temps polynomial. $\square$

> **Notion de coNP** : Un langage $L$ est dans **coNP** si son complément $\bar{L}$ est dans NP. On a $\mathsf{P} \subseteq \mathsf{NP} \cap \mathsf{coNP}$. La question $\mathsf{NP} = \mathsf{coNP}$ est également ouverte.

---

## 13. Exercices sur la NP-complétude

### Rappel méthodologique

Pour montrer qu'un langage $L$ est **NP-complet**, on procède en **deux étapes** :

> **Étape 1** : Montrer que $L \in \text{NP}$ (exhiber un certificat et un vérificateur polynomial).
>
> **Étape 2** : Montrer qu'un problème NP-complet **connu** se réduit polynomialement à $L$ : trouver $L' \in \text{NPC}$ tel que $L' \leq_{\mathsf{P}} L$.

---

### Exercice 1 (Démo 9) — Temps de calcul et classification

**Énoncé** : Soit $M$ une MT telle que :
- Sur entrée $w$ commençant par 0 : accepte automatiquement
- Sur entrée $w$ commençant par 1 : écrit $0^{2^{|w|}}$ puis refuse

Questions :
1. Quel est le temps de calcul de $M(01010101)$ ? Et de $M(10101010)$ ?
2. Est-ce que $L(M) \in \mathsf{P}$, $\mathsf{NP}$, ou $\mathsf{EXPTIME}$ ?

**Solution** :

**Temps de calcul** :

| Entrée | Commence par | Comportement | Temps |
|---|---|---|---|
| $01010101$ | $0$ | Accepte immédiatement | $O(1)$ — **constant** |
| $10101010$ | $1$ | Écrit $0^{2^8} = 0^{256}$ puis refuse | $O(2^8) = O(256)$ — **exponentiel** en $|w| = 8$ |

**Classification de $L(M)$** :

$$L(M) = \{w \in \Sigma^* \mid w \text{ commence par } 0\}$$

Ce langage est **régulier** (reconnu par un AFD à 2 états) et donc :

$$L(M) \in \mathsf{P} \subseteq \mathsf{NP} \subseteq \mathsf{EXPTIME}$$

> **Point subtil** : Le fait que $M$ prenne un temps exponentiel sur certaines entrées ne signifie pas que le **langage** $L(M)$ est difficile. Il existe une **autre** MT $M'$ qui décide le même langage en temps $O(1)$. La complexité d'un langage est définie par la **meilleure** MT qui le décide, pas par une MT particulière.

---

### Exercice 2 (Démo 9) — 5-SAT est NP-complet

**Énoncé** : Montrer que $5\text{-SAT}$ est NP-complet, où :

$$5\text{-SAT} = \{\langle \phi \rangle \mid \phi \text{ est une expression satisfaisable en 5-FNC}\}$$

(Une expression en **5-FNC** est une conjonction de clauses, chaque clause étant une disjonction d'exactement 5 littéraux.)

**Solution** :

**Étape 1 — $5\text{-SAT} \in \text{NP}$** :

- **Certificat** : Une affectation des variables booléennes
- **Vérification** : Évaluer chaque clause (5 littéraux par clause) et vérifier qu'elles sont toutes vraies
- **Temps** : $O(m)$ où $m$ = nombre de clauses → **polynomial**

$\square$

**Étape 2 — $3\text{-SAT} \leq_{\mathsf{P}} 5\text{-SAT}$** :

On sait que $3\text{-SAT}$ est NP-complet. On construit une réduction.

**Fonction de réduction** $f$ : Soit $\phi$ une instance de $3\text{-SAT}$ avec des clauses $(t_1 \vee t_2 \vee t_3)$.

Pour chaque clause $(t_1 \vee t_2 \vee t_3)$, on la transforme en clause à 5 littéraux en ajoutant deux nouvelles variables auxiliaires $y, z$ :

$$(t_1 \vee t_2 \vee t_3) \mapsto (t_1 \vee t_2 \vee t_3 \vee y \vee z) \wedge (t_1 \vee t_2 \vee t_3 \vee y \vee \neg z) \wedge (t_1 \vee t_2 \vee t_3 \vee \neg y \vee z) \wedge (t_1 \vee t_2 \vee t_3 \vee \neg y \vee \neg z)$$

**Correction** :
- Si $(t_1 \vee t_2 \vee t_3)$ est vraie, alors les 4 clauses sont vraies (quel que soit $y, z$)
- Si $(t_1 \vee t_2 \vee t_3)$ est fausse, alors aucune valeur de $(y, z)$ ne rend les 4 clauses simultanément vraies (car on a les 4 combinaisons)

**Temps** : La transformation est linéaire en la taille de $\phi$ → **polynomiale**.

**Conclusion** : $5\text{-SAT}$ est NP-complet. $\square$

> **Généralisation** : Pour tout $k \geq 3$, le problème $k\text{-SAT}$ est NP-complet. En revanche, $2\text{-SAT} \in \mathsf{P}$.

---

### Exercice 3 (Démo 9) — ANTICLIQUE est NP-complet

**Énoncé** : Montrer que $\text{ANTICLIQUE}$ est NP-complet, où :

$$\text{ANTICLIQUE} = \{\langle G, k \rangle \mid G \text{ a une anticlique de taille } k, k \geq 1\}$$

**Solution** :

**Étape 1 — ANTICLIQUE $\in$ NP** :

- **Certificat** : Un sous-ensemble $S$ de $k$ sommets de $G$
- **Vérification** : Pour chaque paire $(u, v) \in S \times S$, vérifier que $(u, v) \notin E$
- **Temps** : $O(k^2) \leq O(n^2)$ → **polynomial**

$\square$

**Étape 2 — $\text{CLIQUE} \leq_{\mathsf{P}} \text{ANTICLIQUE}$** :

On sait que CLIQUE est NP-complet. On utilise la notion de **complément de graphe**.

**Rappel** (Définition 5.63) : Le complément $\bar{G}$ d'un graphe $G = (V, E)$ est le graphe $(V, \bar{E})$ où $\bar{E} = \{(u,v) \mid u \neq v, (u,v) \notin E\}$.

**Fonction de réduction** : $f(\langle G, k \rangle) = \langle \bar{G}, k \rangle$

**Correction** :

$$\langle G, k \rangle \in \text{CLIQUE} \iff G \text{ contient une clique de taille } k$$
$$\iff \bar{G} \text{ contient une anticlique de taille } k$$
$$\iff \langle \bar{G}, k \rangle \in \text{ANTICLIQUE}$$

**Justification** : Un ensemble $S$ de sommets forme une clique dans $G$ (toutes les paires sont reliées) si et seulement si ce même ensemble forme une anticlique dans $\bar{G}$ (aucune paire n'est reliée dans $\bar{G}$).

**Temps** : Construire $\bar{G}$ prend $O(n^2)$ → **polynomial**.

**Conclusion** : ANTICLIQUE est NP-complet. $\square$

---

### Exercice 4 (Démo 9) — 5COL est NP-complet

**Énoncé** : Montrer que $5\text{COL}$ est NP-complet, où :

$$5\text{COL} = \{\langle G \rangle \mid G \text{ est 5-coloriable}\}$$

**Solution** :

**Étape 1 — $5\text{COL} \in \text{NP}$** :

- **Certificat** : Un 5-coloriage, i.e., une fonction $c : V \to \{1, 2, 3, 4, 5\}$
- **Vérification** : Pour chaque arête $(u, v) \in E$, vérifier que $c(u) \neq c(v)$
- **Temps** : $O(|E|) \leq O(n^2)$ → **polynomial**

$\square$

**Étape 2 — $3\text{-COL} \leq_{\mathsf{P}} 5\text{COL}$** :

On sait que $3\text{-COL}$ est NP-complet. On construit une réduction.

**Fonction de réduction** : Étant donné un graphe $G = (V, E)$, on construit $G' = (V', E')$ :

$$V' = V \cup \{a, b\} \quad \text{(2 nouveaux sommets)}$$

$$E' = E \cup \{(a, b)\} \cup \{(a, v) \mid v \in V\} \cup \{(b, v) \mid v \in V\}$$

Les sommets $a$ et $b$ sont reliés entre eux et reliés à **tous** les sommets de $G$.

**Correction** :

- **($\Rightarrow$)** Si $G$ est 3-coloriable avec les couleurs $\{1, 2, 3\}$, alors on colorie $a$ avec la couleur 4 et $b$ avec la couleur 5. Comme $a$ et $b$ sont adjacents à tous les sommets de $V$ et entre eux, et que les sommets de $V$ n'utilisent que les couleurs $\{1, 2, 3\}$, on obtient un 5-coloriage valide de $G'$.

- **($\Leftarrow$)** Si $G'$ est 5-coloriable, alors $a$ et $b$ ont des couleurs distinctes (car adjacents). Puisque chaque sommet $v \in V$ est adjacent à $a$ et à $b$, aucun sommet de $V$ ne peut utiliser les couleurs de $a$ ou de $b$. Donc les sommets de $V$ utilisent au plus $5 - 2 = 3$ couleurs, et le coloriage restreint à $V$ est un 3-coloriage valide de $G$.

**Temps** : $O(n)$ pour construire $G'$ → **polynomial**.

**Conclusion** : $5\text{COL}$ est NP-complet. $\square$

> **Généralisation** : Pour tout $k \geq 3$, le problème $k\text{-COL}$ est NP-complet. La réduction de $3\text{-COL}$ à $k\text{-COL}$ s'effectue en ajoutant $k - 3$ sommets supplémentaires tous reliés entre eux et à tous les sommets du graphe original.

---

### Exercice 5 (Démo 9) — COUVERTURE est NP-complet

**Énoncé** : Montrer que $\text{COUVERTURE}$ est NP-complet, où :

$$\text{COUVERTURE} = \{\langle G, k \rangle \mid G \text{ possède une couverture de sommets de taille } k\}$$

**Solution** :

**Étape 1 — COUVERTURE $\in$ NP** :

- **Certificat** : Un sous-ensemble $S \subseteq V$ de taille $k$
- **Vérification** : Pour chaque arête $(u, v) \in E$, vérifier que $u \in S$ ou $v \in S$
- **Temps** : $O(|E| \cdot k) \leq O(n^3)$ → **polynomial**

$\square$

**Étape 2 — $\text{CLIQUE} \leq_{\mathsf{P}} \text{COUVERTURE}$** :

**Lemme fondamental** : Soit $G = (V, E)$ un graphe à $n$ sommets et $S \subseteq V$. Alors :

$$S \text{ est une clique dans } G \iff V \setminus S \text{ est une couverture dans } \bar{G}$$

**Fonction de réduction** : $f(\langle G, k \rangle) = \langle \bar{G}, n - k \rangle$

**Correction** :

$$\langle G, k \rangle \in \text{CLIQUE}$$
$$\iff G \text{ contient une clique } S \text{ de taille } k$$
$$\iff V \setminus S \text{ est une couverture de taille } n - k \text{ dans } \bar{G}$$
$$\iff \langle \bar{G}, n - k \rangle \in \text{COUVERTURE}$$

**Preuve du lemme** :

- Si $S$ est une clique dans $G$ : pour toute arête $(u, v)$ dans $\bar{G}$, on a $(u, v) \notin E$. Donc $u$ et $v$ ne sont pas tous les deux dans $S$ (sinon ils seraient reliés dans $G$). Donc au moins l'un des deux est dans $V \setminus S$, ce qui signifie que $V \setminus S$ couvre l'arête.

- Réciproquement, si $V \setminus S$ est une couverture dans $\bar{G}$ : pour toute paire $u, v \in S$, l'arête $(u, v)$ doit être dans $E$ (sinon $(u, v) \in \bar{E}$ et ni $u$ ni $v$ n'est dans $V \setminus S$, ce qui contredirait la couverture). Donc $S$ est une clique.

**Temps** : $O(n^2)$ pour construire $\bar{G}$ → **polynomial**.

**Conclusion** : COUVERTURE est NP-complet. $\square$

**Remarque 5.65** : On a aussi $\text{COUVERTURE} \leq_{\mathsf{P}} \text{CLIQUE}$ par la transformation inverse, ce qui montre que les deux problèmes sont **polynomialement équivalents**.

---

## Tableau récapitulatif des réductions utilisées dans les exercices des démonstrations

```
       SAT-C (Cook-Levin)
         │
         ▼
        SAT
         │
         ▼
      SAT-FNC
         │
         ▼
       3-SAT ──────────────────┐
      ╱  │  ╲                  │
     ╱   │   ╲                 │
    ▼    ▼    ▼                ▼
CLIQUE  3-COL  PROGR-ENT    5-SAT
  │  ╲    │
  │   ╲   ▼
  │    ╲ HORAIRE
  │     ╲
  ▼      ▼
ANTICL. COUVERTURE
  ↕        ↕
CLIQUE  CLIQUE

     3-COL → 5-COL → k-COL (k ≥ 3)
```

---
---

# PARTIE III — 5 Exercices Supplémentaires avec Corrections

---

## Exercice Supplémentaire 1 — Lemme de pompage (langage hors-contexte)

**Énoncé** : Démontrez que le langage suivant n'est pas hors-contexte :

$$L = \{a^n b^n c^n d^n \mid n \geq 0\}$$

---

### Correction détaillée

**Méthode** : Raisonnement par l'absurde en utilisant le **lemme de pompage pour les langages hors-contexte**.

**Rappel du lemme** : Si $L \in \text{HC}$, alors il existe $p \geq 1$ (longueur de pompe) tel que tout mot $w \in L$ avec $|w| \geq p$ peut s'écrire $w = uvxyz$ avec :
- $|vxy| \leq p$
- $|vy| \geq 1$
- $\forall i \geq 0 : uv^i xy^i z \in L$

**Preuve** :

Supposons par l'absurde que $L \in \text{HC}$ avec longueur de pompe $p$.

**Choix du mot** : Soit $w = a^p b^p c^p d^p$. On a $w \in L$ et $|w| = 4p \geq p$.

Par le lemme, on peut écrire $w = uvxyz$ avec $|vxy| \leq p$ et $|vy| \geq 1$.

**Observation clé** : Puisque $|vxy| \leq p$, la sous-chaîne $vxy$ ne peut couvrir **au plus que deux types consécutifs** de symboles parmi $\{a, b, c, d\}$.

**Analyse par cas** — on pompe avec $i = 2$ (i.e., on considère $uv^2xy^2z$) :

| Cas | Symboles dans $vxy$ | Effet du pompage ($i=2$) | Contradiction |
|---|---|---|---|
| 1 | Seulement des $a$ | Le nombre de $a$ augmente, les autres restent $p$ | $\#a > \#b = \#c = \#d$ → $\notin L$ |
| 2 | Des $a$ et des $b$ | $\#a$ ou $\#b$ augmente (ou les deux), $\#c = \#d = p$ | Les quatre quantités ne sont plus égales → $\notin L$ |
| 3 | Seulement des $b$ | Le nombre de $b$ augmente | $\#b > \#a = \#c = \#d$ → $\notin L$ |
| 4 | Des $b$ et des $c$ | $\#b$ ou $\#c$ augmente, $\#a = \#d = p$ | → $\notin L$ |
| 5 | Seulement des $c$ | Le nombre de $c$ augmente | → $\notin L$ |
| 6 | Des $c$ et des $d$ | $\#c$ ou $\#d$ augmente, $\#a = \#b = p$ | → $\notin L$ |
| 7 | Seulement des $d$ | Le nombre de $d$ augmente | → $\notin L$ |

> **Point crucial** : $vxy$ ne peut **jamais** chevaucher les quatre symboles à la fois (car $|vxy| \leq p$ et les blocs sont de taille $p$ chacun). Donc le pompage ne peut augmenter qu'**au plus deux** des quatre compteurs, brisant l'égalité.

Dans **tous les cas**, $uv^2xy^2z \notin L$, ce qui contredit le lemme de pompage.

**Conclusion** : $L \notin \text{HC}$. $\square$

---

## Exercice Supplémentaire 2 — Appartenance à NP

**Énoncé** : Soit le langage :

$$\text{PARTITION} = \left\{\langle S \rangle \;\middle|\; S = \{x_1, \ldots, x_m\} \subset \mathbb{N}, \;\exists\, A \subseteq S \text{ t.q. } \sum_{x \in A} x = \sum_{x \in S \setminus A} x \right\}$$

**(a)** Montrez que $\text{PARTITION} \in \text{NP}$.

**(b)** Décrivez informellement pourquoi on ne connaît pas d'algorithme polynomial évident pour ce problème.

---

### Correction détaillée

**(a) PARTITION $\in$ NP** :

**Certificat (témoin)** : Un sous-ensemble $A \subseteq S$.

**Taille du certificat** : $|A| \leq |S| = m$ → taille polynomiale en la taille de l'entrée.

**Vérificateur** $V(\langle S \rangle, A)$ :

```
Algorithme V(S, A) :
1. Pour chaque x ∈ A :
     Vérifier que x ∈ S                          → O(m) par élément
2. Calculer s_A = Σ_{x ∈ A} x                   → O(m)
3. Calculer s_total = Σ_{x ∈ S} x               → O(m)
4. Si s_A = s_total - s_A (i.e., s_A = s_total / 2) :
     ACCEPTER
   Sinon :
     REJETER
```

**Analyse de la correction** :

- **Si $\langle S \rangle \in \text{PARTITION}$** : il existe un sous-ensemble $A$ qui partitionne $S$ en deux parties de même somme. En choisissant ce $A$ comme certificat, le vérificateur accepte. ✓
- **Si $\langle S \rangle \notin \text{PARTITION}$** : pour tout sous-ensemble $A \subseteq S$, on a $\sum_{x \in A} x \neq \sum_{x \in S \setminus A} x$, donc le vérificateur rejette. ✓
- **Temps** : $O(m^2)$ au total → **polynomial**. ✓

**Conclusion** : $\text{PARTITION} \in \text{NP}$. $\square$

**(b) Pourquoi pas d'algorithme polynomial évident ?**

L'approche naïve consisterait à tester **tous les sous-ensembles** $A \subseteq S$. Il y en a $2^m$, ce qui donne un temps **exponentiel** $O(m \cdot 2^m)$.

On ne connaît aucune stratégie fondamentalement meilleure. En fait, PARTITION est un cas particulier de SUBSET-SUM, qui est **NP-complet**. PARTITION est lui-même NP-complet (ce qui peut être démontré par réduction depuis SUBSET-SUM).

> **Remarque** : Il existe un algorithme de programmation dynamique en $O(m \cdot T)$ où $T = \sum x_i$, mais $T$ peut être **exponentiellement grand** par rapport à la taille de l'encodage (qui est en $O(m \log T)$). C'est un algorithme **pseudo-polynomial**, pas polynomial.

---

## Exercice Supplémentaire 3 — NP-complétude par réduction depuis 3-SAT

**Énoncé** : Soit le langage :

$$\text{DOUBLE-SAT} = \left\{\langle \phi \rangle \;\middle|\; \phi \text{ est une expression booléenne en FNC possédant au moins } \textbf{deux} \text{ affectations satisfaisantes distinctes}\right\}$$

Montrez que $\text{DOUBLE-SAT}$ est NP-complet.

---

### Correction détaillée

#### Étape 1 — DOUBLE-SAT $\in$ NP

**Certificat** : Deux affectations distinctes $\alpha_1$ et $\alpha_2$ des variables de $\phi$.

**Vérificateur** $V(\langle \phi \rangle, (\alpha_1, \alpha_2))$ :

```
Algorithme V(φ, (α₁, α₂)) :
1. Vérifier que α₁ ≠ α₂                           → O(n)
2. Évaluer φ sous l'affectation α₁                → O(|φ|)
3. Évaluer φ sous l'affectation α₂                → O(|φ|)
4. Si φ(α₁) = VRAI et φ(α₂) = VRAI : ACCEPTER
   Sinon : REJETER
```

**Analyse** :
- Taille du certificat : $2n$ bits (deux affectations de $n$ variables) → **polynomiale**
- Temps : $O(|\phi|)$ → **polynomial**
- Correction : immédiate

$\Rightarrow$ $\text{DOUBLE-SAT} \in \text{NP}$. $\square$

#### Étape 2 — $3\text{-SAT} \leq_{\mathsf{P}} \text{DOUBLE-SAT}$

**Idée** : À partir d'une formule $\phi$ en 3-FNC sur les variables $x_1, \ldots, x_n$, on construit une formule $\phi'$ qui a **au moins 2** affectations satisfaisantes si et seulement si $\phi$ est satisfaisable.

**Construction de la réduction** :

On introduit une **nouvelle variable** $y$ n'apparaissant pas dans $\phi$, et on pose :

$$f(\langle \phi \rangle) = \langle \phi' \rangle \quad \text{où} \quad \phi' = \phi(x_1, \ldots, x_n) \wedge (y \vee \neg y)$$

avec $y$ nouvelle variable. Ici $\phi'$ a pour variables $x_1, \ldots, x_n, y$.

**Correction** :

**($\Rightarrow$)** Supposons $\phi$ satisfaisable. Soit $\alpha = (a_1, \ldots, a_n)$ une affectation satisfaisante de $\phi$. Alors :
- $\alpha_1 = (a_1, \ldots, a_n, y = 0)$ satisfait $\phi'$
- $\alpha_2 = (a_1, \ldots, a_n, y = 1)$ satisfait $\phi'$

Et $\alpha_1 \neq \alpha_2$ (elles diffèrent sur $y$). Donc $\phi'$ a au moins 2 affectations satisfaisantes distinctes.

$\Rightarrow \langle \phi' \rangle \in \text{DOUBLE-SAT}$. ✓

**($\Leftarrow$)** Supposons $\langle \phi' \rangle \in \text{DOUBLE-SAT}$. Alors $\phi'$ est satisfaisable, donc en particulier il existe une affectation des variables $x_1, \ldots, x_n, y$ qui rend $\phi'$ vraie. Puisque $\phi' = \phi \wedge (y \vee \neg y)$, cela implique que $\phi$ est satisfaisable (la clause $(y \vee \neg y)$ est toujours vraie).

$\Rightarrow \langle \phi \rangle \in 3\text{-SAT}$. ✓

**Temps de la réduction** : $O(|\phi|)$ → **polynomial**.

**Conclusion** : $\text{DOUBLE-SAT}$ est NP-complet. $\square$

---

## Exercice Supplémentaire 4 — NP-complétude de ENSEMBLE DOMINANT

**Énoncé** : Dans un graphe $G = (V, E)$, un **ensemble dominant** est un sous-ensemble $D \subseteq V$ tel que tout sommet $v \notin D$ a au moins un voisin dans $D$. Soit :

$$\text{DOMINANT} = \{\langle G, k \rangle \mid G \text{ possède un ensemble dominant de taille } \leq k\}$$

**(a)** Montrez que $\text{DOMINANT} \in \text{NP}$.

**(b)** Montrez que $\text{COUVERTURE} \leq_{\mathsf{P}} \text{DOMINANT}$, et concluez que DOMINANT est NP-complet.

---

### Correction détaillée

#### (a) DOMINANT $\in$ NP

**Certificat** : Un sous-ensemble $D \subseteq V$ avec $|D| \leq k$.

**Vérificateur** $V(\langle G, k \rangle, D)$ :

```
Algorithme V(G, k, D) :
1. Vérifier que |D| ≤ k                            → O(n)
2. Vérifier que D ⊆ V                             → O(n)
3. Pour chaque sommet v ∈ V \ D :
     Vérifier qu'il existe u ∈ D tel que (v, u) ∈ E  → O(n·k)
4. Si toutes les vérifications passent : ACCEPTER
   Sinon : REJETER
```

**Temps** : $O(n \cdot k) \leq O(n^2)$ → **polynomial**.

$\Rightarrow$ $\text{DOMINANT} \in \text{NP}$. $\square$

#### (b) COUVERTURE $\leq_{\mathsf{P}}$ DOMINANT

**Construction de la réduction** :

Soit $\langle G, k \rangle$ une instance de COUVERTURE, avec $G = (V, E)$.

On construit un graphe $G' = (V', E')$ comme suit :

- **Sommets** : Pour chaque arête $e = (u, v) \in E$, on ajoute un **nouveau sommet** $w_e$ :

$$V' = V \cup \{w_e \mid e \in E\}$$

- **Arêtes** : On conserve les arêtes de $G$, et pour chaque arête $e = (u, v) \in E$, on ajoute les arêtes $(w_e, u)$ et $(w_e, v)$ :

$$E' = E \cup \{(w_e, u), (w_e, v) \mid e = (u,v) \in E\}$$

On pose $f(\langle G, k \rangle) = \langle G', k \rangle$.

**Correction** :

- **($\Rightarrow$)** Supposons que $G$ possède une couverture $C \subseteq V$ de taille $\leq k$. Montrons que $C$ est un ensemble dominant dans $G'$.

  - Pour tout sommet $v \in V \setminus C$ : puisque $C$ est une couverture, toute arête incidente à $v$ a son autre extrémité dans $C$. Si $v$ a au moins un voisin, ce voisin est dans $C$. Si $v$ est isolé dans $G$, il n'y a pas de sommet $w_e$ associé à une arête incidente à $v$, donc $v$ n'a aucun nouveau voisin non plus — mais un sommet isolé n'apparaît pas dans une arête, donc ce cas est géré.

    En fait, si $v \in V \setminus C$ a un voisin $u$ dans $G$, alors $(v, u) \in E$ et $u \in C$ (car $C$ couvre l'arête). Donc $v$ est dominé par $u \in C$.

  - Pour tout sommet $w_e \in V' \setminus V$ avec $e = (u, v)$ : puisque $C$ couvre l'arête $e$, au moins l'un de $u, v$ est dans $C$, et $w_e$ est adjacent à $u$ et $v$. Donc $w_e$ est dominé.

  $\Rightarrow C$ est un ensemble dominant de taille $\leq k$ dans $G'$. ✓

- **($\Leftarrow$)** Supposons que $G'$ possède un ensemble dominant $D$ de taille $\leq k$. On peut supposer sans perte de généralité que $D \subseteq V$ (si $w_e \in D$ pour $e = (u,v)$, on peut remplacer $w_e$ par $u$ ou $v$ sans augmenter la taille ni perdre la propriété de domination, car les voisins de $w_e$ sont $u$ et $v$, et tout ce que $w_e$ domine est aussi voisin de $u$ ou $v$).

  Soit $D \subseteq V$ un tel ensemble dominant. Pour toute arête $e = (u, v) \in E$, le sommet $w_e$ doit être dominé, donc au moins l'un de $u, v$ est dans $D$. Cela signifie exactement que $D$ est une couverture de $G$. ✓

**Temps** : La construction de $G'$ se fait en $O(|V| + |E|)$ → **polynomial**.

**Conclusion** : Puisque COUVERTURE est NP-complet et $\text{COUVERTURE} \leq_{\mathsf{P}} \text{DOMINANT}$, et que $\text{DOMINANT} \in \text{NP}$, on conclut que **DOMINANT est NP-complet**. $\square$

---

## Exercice Supplémentaire 5 — Complexité et coNP

**Énoncé** : Soit le langage :

$$\text{TAUTOLOGIE} = \{\langle \phi \rangle \mid \phi \text{ est une expression booléenne en FNC vraie pour \textbf{toute} affectation}\}$$

**(a)** Montrez que $\text{TAUTOLOGIE} \in \text{coNP}$.

**(b)** Montrez que si $\text{TAUTOLOGIE} \in \text{NP}$, alors $\text{NP} = \text{coNP}$.

**(c)** Soit $M$ une machine de Turing qui, sur entrée $\langle \phi \rangle$ avec $\phi$ en FNC sur $n$ variables, teste exhaustivement les $2^n$ affectations possibles et accepte si toutes rendent $\phi$ vraie. Donnez la complexité en temps de $M$ et déterminez dans quelle classe de complexité cette analyse place le langage.

---

### Correction détaillée

#### (a) TAUTOLOGIE $\in$ coNP

**Rappel** : $L \in \text{coNP}$ signifie $\bar{L} \in \text{NP}$.

On identifie le complément :

$$\overline{\text{TAUTOLOGIE}} = \{\langle \phi \rangle \mid \phi \text{ n'est PAS une tautologie}\} = \{\langle \phi \rangle \mid \exists \text{ une affectation } \alpha \text{ t.q. } \phi(\alpha) = \text{FAUX}\}$$

**Montrons que $\overline{\text{TAUTOLOGIE}} \in \text{NP}$** :

**Certificat** : Une affectation $\alpha$ des variables de $\phi$.

**Vérificateur** $V(\langle \phi \rangle, \alpha)$ :

```
Algorithme V(φ, α) :
1. Évaluer φ sous l'affectation α                 → O(|φ|)
2. Si φ(α) = FAUX : ACCEPTER (car φ n'est pas une tautologie)
   Sinon : REJETER
```

**Analyse** :
- Taille du certificat : $n$ bits → polynomiale
- Temps : $O(|\phi|)$ → polynomial
- Correction : $\phi$ n'est pas une tautologie $\iff$ $\exists \alpha : \phi(\alpha) = \text{FAUX}$ $\iff$ $\exists$ un certificat accepté ✓

$\Rightarrow \overline{\text{TAUTOLOGIE}} \in \text{NP}$, donc $\text{TAUTOLOGIE} \in \text{coNP}$. $\square$

#### (b) Si TAUTOLOGIE $\in$ NP, alors NP $=$ coNP

**Preuve** :

Supposons $\text{TAUTOLOGIE} \in \text{NP}$.

**Étape 1** : Montrons que $\text{TAUTOLOGIE}$ est **coNP-complet** (i.e., tout langage de coNP s'y réduit polynomialement).

Soit $L \in \text{coNP}$. Alors $\bar{L} \in \text{NP}$. Puisque SAT est NP-complet, on a $\bar{L} \leq_{\mathsf{P}} \text{SAT}$ via une réduction $f$.

Or, pour toute formule $\phi$ :
$$\phi \in \text{SAT} \iff \neg\phi \notin \text{TAUTOLOGIE}$$

Plus précisément, considérons la réduction $g(w) = \neg f(w)$ (on nie la formule obtenue). Alors :
$$w \in L \iff w \notin \bar{L} \iff f(w) \notin \text{SAT} \iff \neg f(w) \in \text{TAUTOLOGIE} \iff g(w) \in \text{TAUTOLOGIE}$$

Donc $L \leq_{\mathsf{P}} \text{TAUTOLOGIE}$, ce qui confirme que TAUTOLOGIE est coNP-complet.

**Étape 2** : Si $\text{TAUTOLOGIE} \in \text{NP}$, alors pour tout $L \in \text{coNP}$ :

$$L \leq_{\mathsf{P}} \text{TAUTOLOGIE} \in \text{NP} \implies L \in \text{NP}$$

Donc $\text{coNP} \subseteq \text{NP}$.

**Étape 3** : Par symétrie (en prenant le complément), on obtient $\text{NP} \subseteq \text{coNP}$.

**Conclusion** : $\text{NP} = \text{coNP}$. $\square$

> **Remarque** : La majorité des chercheurs conjecturent que $\text{NP} \neq \text{coNP}$, ce qui impliquerait $\text{TAUTOLOGIE} \notin \text{NP}$.

#### (c) Complexité de la machine exhaustive

**Analyse de $M$** :

La machine $M$ effectue les opérations suivantes sur une formule $\phi$ à $n$ variables et $m$ clauses :

```
Pour chaque affectation α ∈ {0,1}^n :     → 2^n itérations
   Évaluer φ(α)                             → O(m) par évaluation
   Si φ(α) = FAUX : REJETER
Si toutes les affectations rendent φ vraie : ACCEPTER
```

**Temps de calcul** :

$$T(n, m) = O(2^n \cdot m)$$

Si la taille de l'entrée est $N = |\langle \phi \rangle| \in O(n \cdot m)$, alors $n \leq N$ et :

$$T(N) \in O(2^N \cdot N)$$

**Classification** :

$$T(N) \in O(2^N \cdot N) \subseteq O(2^{N^2}) \in O(2^{N^k}) \text{ pour } k = 2$$

Donc cette analyse place TAUTOLOGIE dans :

$$\text{TAUTOLOGIE} \in \text{EXPTIME} = \bigcup_{k \geq 0} \text{TIME}(2^{n^k})$$

Ceci est cohérent avec la chaîne d'inclusions connue :

$$\text{P} \subseteq \text{NP} \subseteq \text{coNP}^{(?)} \subseteq \text{EXPTIME}$$

et ne nous dit rien de plus que ce qu'on savait déjà (puisque $\text{coNP} \subseteq \text{EXPTIME}$). $\square$

---

## Tableau récapitulatif des 5 exercices supplémentaires

| # | Thème | Technique clé | Résultat |
|---|---|---|---|
| **1** | $\{a^n b^n c^n d^n\} \notin \text{HC}$ | Lemme de pompage HC | $vxy$ ne couvre que $\leq 2$ symboles |
| **2** | PARTITION $\in$ NP | Certificat = sous-ensemble $A$ | Vérification en $O(m^2)$ |
| **3** | DOUBLE-SAT NP-complet | $3\text{-SAT} \leq_{\mathsf{P}} \text{DOUBLE-SAT}$ | Ajout d'une variable libre $y$ |
| **4** | DOMINANT NP-complet | $\text{COUVERTURE} \leq_{\mathsf{P}} \text{DOMINANT}$ | Ajout de sommets par arête |
| **5** | TAUTOLOGIE $\in$ coNP | $\overline{\text{TAUTOLOGIE}} \in \text{NP}$ | Certificat = contre-exemple |

---
---

# PARTIE IV — Synthèse Méthodologique Globale

---

## Comment montrer qu'un langage est dans NP

| Étape | Action | Vérification |
|---|---|---|
| 1 | Identifier le **certificat** $c$ | $|c|$ est polynomial en $|w|$ ? |
| 2 | Décrire le **vérificateur** $V(w, c)$ | $V$ s'exécute en temps polynomial ? |
| 3 | Prouver la **correction** | $w \in L \Rightarrow \exists c : V$ accepte ? $w \notin L \Rightarrow \forall c : V$ rejette ? |

## Comment montrer qu'un langage est NP-complet

| Étape | Action | Détails |
|---|---|---|
| 1 | Montrer $L \in \text{NP}$ | Voir ci-dessus |
| 2 | Choisir $L'$ NP-complet connu | $3\text{-SAT}$, $\text{CLIQUE}$, $3\text{-COL}$, etc. |
| 3 | Construire la réduction $f$ | $f$ calculable en temps polynomial |
| 4 | Prouver $x \in L' \iff f(x) \in L$ | Sens $\Rightarrow$ et sens $\Leftarrow$ |

## Comment utiliser le lemme de pompage (HC)

| Étape | Action |
|---|---|
| 1 | Supposer $L \in \text{HC}$ avec longueur de pompe $p$ |
| 2 | Choisir un mot $w \in L$ avec $|w| \geq p$ |
| 3 | Écrire $w = uvxyz$ avec $|vxy| \leq p$, $|vy| \geq 1$ |
| 4 | Montrer que $\exists i : uv^i xy^i z \notin L$ pour **tous** les découpages possibles |
| 5 | Conclure par contradiction |

## Erreurs fréquentes à éviter

1. **Réduire dans le mauvais sens** : Pour montrer que $L$ est NP-complet, il faut réduire un problème **connu comme NP-complet** VERS $L$ (et non l'inverse).

2. **Oublier de montrer $L \in \text{NP}$** : Un langage peut être NP-difficile sans être dans NP (donc pas NP-complet).

3. **Confondre complexité du langage et de la machine** : Un langage peut être dans P même si une MT particulière le décide en temps exponentiel (cf. Exercice 1, Démo 9).

4. **Pour le lemme de pompage HC** : Il faut couvrir **tous les cas** de position de $vxy$ (début, milieu, fin) et ne pas oublier la contrainte $|vxy| \leq p$.

5. **Oublier de vérifier les deux sens de la réduction** : Il faut toujours prouver $x \in L' \Rightarrow f(x) \in L$ **ET** $x \notin L' \Rightarrow f(x) \notin L$ (ou de manière équivalente, les deux sens de l'équivalence).

---

> **Conclusion générale** : Ce support couvre l'ensemble du chapitre sur les classes P et NP, depuis les définitions fondamentales (machines de Turing, complexité en temps) jusqu'aux techniques avancées (réductions polynomiales, théorème de Cook-Levin, preuves de NP-complétude). Les exercices résolus — issus des démonstrations 8 et 9 ainsi que les 5 exercices supplémentaires — illustrent les techniques fondamentales : lemme de pompage pour les langages hors-contexte, preuve d'appartenance à NP via certificats et vérificateurs polynomiaux, et preuve de NP-complétude via réductions polynomiales. La maîtrise de ces techniques est essentielle pour classifier les problèmes computationnels selon leur difficulté intrinsèque.
```
