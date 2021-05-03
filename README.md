<!--
author:   Marco Hamann

email:    marco.hamann@htw-dresden.de

version:  0.0.1

language: de

comment:  Dieser Kurs richtet sich an Studierende der Hochschule für Technik und Wirtschaft Dresden im Studiengang Medieninformatik im 2. Semester.

script: https://cdn.rawgit.com/davidedc/Algebrite/master/dist/algebrite.bundle-for-browser.js
@Algebrite.eval: <script> Algebrite.run(`@input`)</script>

script:   https://cdn.jsdelivr.net/gh/liaTemplates/tiny-turtle/tiny-turtle.js
@TinyTurtle: @TinyTurtle.eval(@uid,@0,@1,@2)
@TinyTurtle.eval
<script>
send.handle("input", (cmd) => {
  try{
    let rslt = eval(cmd);
    if (rslt)
      console.log(rslt)
  } catch (e) {
    console.error("error", e)
  }
});
function stop() {
  send.lia("LIA: stop")
}
window.TinyTurtle("turtle_@0")
@input
if ("@3" == "true")
  "LIA: terminal"
</script>
<canvas id="turtle_@0" width="@1" height="@2"></canvas>
@end

-->

# Konstruktive Geometrie (I381)

 Dieser Kurs richtet sich an Studierende der Hochschule für Technik und Wirtschaft Dresden im Studiengang Medieninformatik im 2. Semester.

Sie können diesen Kurs auf [LiaScript](https://liascript.github.io/course/?https://github.com/marco-hamann/Geometrie/blob/main/README.md) oder [Opal](https://bildungsportal.sachsen.de/opal/auth/RepositoryEntry/19931103237) aufrufen. Das Repository zu diesem Kurs finden Sie unter

https://github.com/marco-hamann/Geometrie

## Affine Geometrie

In diesem Kapitel werden folgende Themen behandelt:

* Grundlagen zu Vektoren und Matrizen
* Teilverhältnis auf Strecken und affine Koordinaten
* Affine und kartesische Koordinatensysteme
* Schwerpunkt eines Massensystems
* Konvexe Hülle einer Punktmenge
* Unterteilungskurven und Bézier-Kurven

### Vektoren und Matrizen

Vektoren
========

Addition von **Vektoren** aus $\mathbb{R}^k$ $(k\in\mathbb{N})$ und Vielfachbildung von Vektoren mit einem Skalar aus $\mathbb{R}$ sind komponentenweise erklärt.

> **Satz.** Es gelten die folgenden Rechenregeln [^1]
> * Kommutativität $x+y=y+x$
> * Assoziativität $x+(y+z)=(x+y)+z$
> * Neutrales Element $o$ mit $x+o=x$
> * Inverses Element $-{x}$ mit ${x}+(-{x})={o}$
> * Distributivität $\lambda\cdot({x}+{y})=\lambda\cdot{x}+\lambda\cdot{y}$
> * Distributivität $(\lambda+\mu)\cdot{x}=\lambda\cdot{x}+\mu\cdot{x}$
> * Assoziativität für Vielfachbildung $(\lambda\cdot\mu)\cdot{x}=\lambda\cdot(\mu\cdot{x})$
> für beliebige $\{x,y,z\}\subset\mathbb{R}^k$ und $\{\lambda,\mu\}\subset\mathbb{R}$.

``` javascript
A = [[a],[b]]
B = [[c],[d]]
A + B - (B + A)
transpose(A)
```
@Algebrite.eval

Für Vektoren $v_i\in\mathbb{R}^k$ mit $i\in\{1,2,\ldots,n\}$ und Skalare $\lambda_i\in\mathbb{R}$ heißt
$$
  \sum_{i=1}^n{\left(\lambda_i\cdot v_i\right)}\in\mathbb{R}^k
$$
eine **Linearkombination** der Vektoren $v_i$. Diese ist also wieder ein Vektor. Die Vektoren $v_i$ heißen linear unabhängig, falls
$$
  \sum_{i=0}^n{\left(\lambda_i\cdot v_i\right)}=0\;\rightarrow\;(\lambda_0,\lambda_1,\ldots,\lambda_n)=(0,0,\ldots,0)
$$
andernfalls linear abhängig.

Punkte $P$ der Ebene bzw. des dreidimensionalen Raumes werden mit ihren Ortsvektoren bezüglich eines Ortsvektorraumes identifiziert.
$$ P(x,y,z) \leftrightarrow p=\begin{pmatrix} x \\ y \\ z \end{pmatrix} $$
Bezüglich der kanonischen Basis im Ortsvektorraum stellt sich $p$ dar
$$
  p=\begin{pmatrix} x \\ y \\ z \end{pmatrix} =
  x\cdot\begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} +
  y\cdot\begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} +
  z\cdot\begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix}
$$
Die Koordinaten von $P$ sind hierin die Koeffizienten der Linearkombination der Basisvektoren.

Matrizen
========

Die Menge der reellen Matrizen gleichen Typs bilden bezüglich der erklärten Addition und Skalarvielfachbildung einen Vektorraum, genügen also den gleichen Rechengesetzen wie Vektoren (aus $\mathbb{R}^k$).

Für das Rechnen mit **quadratischen Matrizen** benötigen wir des Weiteren *inverse Matrix*, *Determinante* etc.

``` javascript
A=[[a,b],[c,d]]
inv(A)
adj(A)
det(A)
inv(A)-adj(A)/det(A)
```
@Algebrite.eval

Sicher gewusst?
===============

Sie können Ihr Wissen gern bei der Beantwortung der nachstehenden Fragen testen.

**Frage 1.** Eine reelle quadratische Matrix $A$ heißt regulär, falls gilt:

[[ ]] $\det{A}$ ist gleich Null
[[X]] $\det{A}$ ist verschieden von Null
[[X]] Die inverse Matrix $A^{-1}$ zu $A$ existiert.
[[?]] Die inverse Matrix $A^{-1}$ existiert nicht für singuläre Matrizen.
****************************************

Eine quadratische Matrix wird regulär genannt, falls $\det{A}$ nicht den Wert Null besitzt.

Die inverse Matrix $A^{-1}$ berechnet sich mittels
$$
  A^{-1}=\frac{1}{\det{A}}\cdot\mathop{\rm adj}{A}
$$
Diese existiert somit nur zu regulären Matrizen.

****************************************

**Frage 2.** Die Funktionen $f_i:\mathbb{R}\rightarrow\mathbb{R}$ mit
$$
  f_0(x)=1\,,\quad f_1(x)=x\,,\quad f_2(x)=x^2
$$
sind linear unabhängig[^2], da für alle $x$:

[(X)] die nachstehende Implikation gilt $$\sum_{i=0}^2{\left(\lambda_i\cdot f_i(x)\right)}=0\;\rightarrow\;(\lambda_0,\lambda_1,\lambda_2)=(0,0,0)$$
[( )] die Gleichung $$\sum_{i=0}^2{\left(\lambda_i\cdot f_i(x)\right)}=0$$ die Lösung $(\lambda_0,\lambda_1,\lambda_2)=(0,0,0)$ besitzt.
[[?]] Das geordnete Dreitupel $(\lambda_0,\lambda_1,\lambda_2)=(0,0,0)$ ist unabhängig von den gewählten Funktionen immer eine Lösung.
****************************************

Die Null gesetzte Linearkombination ergibt
$$
  \lambda_0+\lambda_1\cdot x+\lambda_2\cdot x^2=0
$$
und ist für alle $x$ genau dann gleich Null, wenn für alle Koeffizienten $\lambda_i=0$ gilt.

****************************************

[^1]: Die Rechenregeln werden zur axiomatischen Festlegung eines Vektorraumes genutzt. Hier ist zusätzlich $1\cdot x=x$ zu fordern.

[^2]: Die Funktionen bilden sogar die Monombasis im Vektorraum aller reellen Polynomfunktionen vom Grad $n\leq 2$.


### Affine Koordinatensysteme

Ziel ist es, die Lage eines Punktes auf einer Geraden / in der Ebene / im Raum in einem geeigneten Koordinatensystem zu beschreiben.

Koordinatensystem auf einer Geraden
===================================

Gegeben sind zwei Punkte $O$ ('Ursprung') sowie $E$ ('Einheitspunkt') auf einer Geraden $g$. Die Strecke $[O,E]$ besitzt die Länge $\overline{OE}=e>0$ ('Einheitslänge').

![Affine Koordinate](img/geo-bild01.png "_Fig._ Teilverhältnis von $X$ zur Strecke $[O,E]$.")

Für einen weiteren Punkt $X\in g$ gilt nach dem Strahlensatz
$$
  \frac{|x|}{1}=\frac{\overline{OX}}{\overline{OE}}\geq 0
$$
worin $|x|$ den Absolutbetrag des reellen Skalenwertes $x$ bezeichnet. Um eine eineindeutige Zuordnung zwischen den Werten $x$ und den Lagen des Punktes $X\in g$ bezüglich der Punkte $O\in g$ und $E\in g$ zu erhalten, wird vereinbart:

| $x>0$ | $x<0$ |
| $O$ trennt die Punkte $E$ und $X$ *nicht* | $O$ trennt die Punkte $E$ und $X$ |

Des Weiteren wird $x=0$ genau für die Lage $X=O$ vereinbart.

> **Definition.** Das Teilverhältnis eines Punktes $X$ auf der Geraden $AB$ ist das vorzeichenbehaftete Verhältnis
> $$
  \lambda=TV(X,B,A)=\epsilon\cdot\frac{\overline{AX}}{\overline{AB}},
  \quad\epsilon=\pm1
$$
> je nachdem $A$ die Punkte $B$ und $X$ trennt oder nicht.
>
> $\lambda$ wird affine Koordinate von $X$ bezüglich $(A,B)$, das Paar $(A,B)$ ein **affines Koordinatensystem** auf $g$ genannt.

> **Satz.** Ein affines Koordinatensystem auf einer Geraden $g$ ist durch zwei verschiedene Punkte $O\in g$ und $E\in g$ bestimmt. Jedem Punkt $X$ der Geraden wird in eineindeutiger Weise als affine Koordinate
> $$
  x=TV(X,E,O)
$$
> zugewiesen.

**Beispiel 1.** Für den Halbierungspunkt $H$ einer Strecke $[A,B]$ gilt nach Definition $$ TV(H,B,A)=\frac{\overline{HA}}{\overline{BA}}\cdot (+1)=\frac{1}{2}$$ Das Teilverhältnis vergleicht die Länge der Teilstrecke $[A,H]$ mit der Länge der Gesamtstrecke $[A,B]$. Das Vorzeichen ist $+1$, da $A$ die Punkte $B$ und $H$ nicht trennt.

Soll ein Punkt $T$ der Strecke $[A,B]$ diese im Verhältnis $1:3$ der Teillängen teilen, so ergeben sich zwei Mögliche Lagen von $T$.[^3]

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 615px;" -->
```````````````````````````````````````````````````````````````````
A                 B     A                 B     A                 B
*--------*--------*     *----*------------*     *------------*----*
         H                   T                               T
```````````````````````````````````````````````````````````````````

Für das Teilverhältnis ergibt sich entsprechend $TV(T,B,A)=\frac{1}{4}$ beziehungsweise $TV(T,B,A)=\frac{3}{4}$. Siehe Abschnitt [Teilungspunkt einer Strecke](#Anwendungen)

**Bemerkung.** Das Teilverhältnis bleibt unter Parallelprojektionen erhalten, d. h. es ändert sich dabei nicht .[^4] Man sagt, es ist invariant unter Parallelprojektionen.

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 415px;" -->
``````````````````````````````````````````````````
     \
--- A *-------------*---------------*---- h
     / \           / B''           / C''
    =   \         =               =
   /     \       /               /
--*-------\-----*---------------*---- g'
  A'       \   / B'            / C'
            \ /               /
             *  B            /
              \             /
               \           /
                \         /
                 \       /
                  \     /
                   \   /
                    \ /
                     * C
                      \
                       g
``````````````````````````````````````````````````

Durch Parallelprojektion werden alle Punkte der Geraden $g$ abgebildet auf Punkte der Geraden $g'$ mithilfe untereinander paralleler Projektionsgeraden
$$
  AA'\,,\quad BB'\,,\quad CC'
$$
Wähle eine Gerade $h$ parallel zu $g'$ durch $A$. Durch Parallelprojektion ergeben sich die Punkte
$$
  B''=BB'\cap h\,,\quad C''=CC'\cap h
$$
mit den Abständen
$$
  \overline{A'B'}=\overline{AB''}\,,\quad \overline{A'C'}=\overline{AC''}
$$
Mit dem Strahlensatz gilt schließlich
$$
  \frac{\overline{AB}}{\overline{AC}}=\frac{\overline{AB''}}{\overline{AC''}}=\frac{\overline{A'B'}}{\overline{A'C'}}
$$
$\square$


Koordinatensystem in einer Ebene
================================

Ein affines Koordinatensystem in einer Ebene lässt sich durch drei Punkte dieser Ebene festlegen, die ein echtes Dreieck bilden. Einer der Punkte sei mit $O$ ('Ursprung'), die beiden anderen mit $E_1$ bzw. $E_2$ ('Einheitspunkte') bezeichnet.

Die Paare $(O,E_1)$ bzw. $(O,E_2)$ bilden jeweils ein affines Koordinatensystem auf
$$
  g_i=O\vee E_i\,,\quad i\in\{1,2\}
$$
die Koordinatenachsen genannt werden. Die Strecken $[O,E_1]$ bzw. $[O,E_2]$ besitzen die Längen $\overline{OE_1}=e_1$ bzw. $\overline{OE_2}=e_2$ ('Einheitslängen').

Jeder Punkt $X$ der Ebene legt vermöge der Geraden $\bar{g}_i$ mit
$$
  \bar{g}_i\parallel g_i \;\wedge\; \bar{g}_i\ni X
$$
ein Punktepaar $(X_1,X_2)$ mit
$$
  X_1 = \bar{g}_2\cap g_1\,,\quad X_2 = \bar{g}_1\cap g_2
$$
fest.

![Affines Koordinatensystem Ebene](img/geo-bild02.png "_Fig._ Affines Koordinatensystem $(O,E_1,E_2)$: Ein Punkt $X$ wird durch die Teilverhältnisse der beiden Koordinatenprojektionen $X_i$ zur Strecke $[O,E_i]$, $i\in\{1,2\}$, umkehrbar eindeutig beschrieben.")

Für $X\not\in g_i$, $i\in\{1,2\}$, bilden $O$ und $X$ die gegenüberliegenden Punkte eines achsenparallelen Parallelogramms mit den Seitenlängen $e_1$ und $e_2$.

> **Satz.** Ein affines Koordinatensystem in einer Ebene $\mathcal{A}^2$ ist durch ein in $\mathcal{A}^2$ liegendes echtes Dreieck mit Eckpunkten $O$, $E_1$ und $E_2$ bestimmt. Jeder Punkt $X\in\mathcal{A}^2$ ist in in umkehrbar eindeutiger Weise durch seine affinen Koordinaten
> $$
  x_1=TV(X_1,E_1,O)\,,\quad x_2=TV(X_2,E_2,O)
$$
> festgelegt.[^1]

Jeder Kantenzug des Parallelogramms von $O$ nach $X$, in dem jede Strecke parallel zu $g_i$ genau einmal vorkommt, heißt dabei ein **Koordinatenweg** von $X$ bezüglich des affinen Koordinatensystems $(O,E_1,E_2)$.

**Bemerkung.** Zur Berechnung des Teilverhältnis von drei Punkten auf einer Geraden $g$ können zweireihige Determinanten zu den Koordinatenvektoren der Punkte verwendet werden.
$$
  TV(C,B,A)=\frac{\det{\begin{pmatrix} x_A & x_C \\ y_A & y_C  \end{pmatrix}}}{\det{\begin{pmatrix} x_A & x_B \\ y_A & y_B \end{pmatrix}}}
$$
Überlegen Sie, in welchen Fällen sich diese Formel verwenden lässt, und in welchen Fällen nicht. Warum beschreibt diese das Teilverhältnis?

``` javascript
A=[xa,ya]
B=[xb,yb]
C=[xc,yc]
zaehler=[[C[1],A[1]],[C[2],A[2]]]
nenner=[[B[1],A[1]],[B[2],A[2]]]
det(zaehler)/det(nenner)
```
@Algebrite.eval


Koordinatensystem im dreidimensionalen Raum
===========================================

Ein affines Koordinatensystem im dreidimensionalen Raum $\mathcal{A}^3$ lässt sich durch vier Punkte dieser Ebene festlegen, die ein echtes Tetraeder bilden. Einer der Punkte sei mit $O$ ('Ursprung'), die anderen mit $E_1$ bzw. $E_2$ bzw. $E_3$ ('Einheitspunkte') bezeichnet.

Die Paare $(O,E_1)$ bzw. $(O,E_2)$ bzw. $(O,E_3)$ bilden jeweils ein affines Koordinatensystem auf
$$
  g_i=O\vee E_i\,,\quad i\in\{1,2,3\}
$$
die Koordinatenachsen genannt werden. Die Strecken $[O,E_1]$ bzw. $[O,E_2]$ bzw. $[O,E_3]$ besitzen die Längen $\overline{OE_1}=e_1$ bzw. $\overline{OE_2}=e_2$ bzw. $\overline{OE_3}=e_3$ ('Einheitslängen').

Jeder Punkt $X$ der Ebene legt vermöge der Ebenen $\epsilon_i$ mit
$$
  \epsilon_i\parallel g_{j}\;\wedge\;\epsilon_i\parallel g_{k}\;\wedge\;\epsilon_i\ni X
$$
und $j\in\{1,2,3\}$, $k\in\{1,2,3\}$ sowie $i\not=j\not=k\not=i$ auf ein Punktetripel $(X_1,X_2,X_3)$ mit
$$
  X_i = \epsilon_i\cap g_i\,,\quad i\in\{1,2,3\}
$$
fest.
Für $X\not\in g_i$, $i\in\{1,2,3\}$, bilden $O$ und $X$ die gegenüberliegenden Punkte eines achsenparallelen [Parallelepipeds](https://de.wikipedia.org/wiki/Parallelepiped "Wikipedia") mit den Kantenlängen $e_1$, $e_2$ und $e_3$.

> **Satz.** Ein affines Koordinatensystem im dreidimensionalen Raum $\mathcal{A}^3$ ist durch ein in $\mathcal{A}^3$ liegendes echtes Tetraeder mit Eckpunkten $O$, $E_1$, $E_2$ und $E_2$ bestimmt. Jeder Punkt $X\in\mathcal{A}^3$ ist in in umkehrbar eindeutiger Weise durch seine affinen Koordinaten
> $$
  x_1=TV(X_1,E_1,O)\,,\quad x_2=TV(X_2,E_2,O)\,,\quad x_3=TV(X_3,E_3,O)
$$
> festgelegt.[^1]

Jeder Kantenzug des Parallelepipeds von $O$ nach $X$, in dem jede Strecke parallel zu $g_i$ genau einmal vorkommt, heißt dabei ein **Koordinatenweg** von $X$ bezüglich des affinen Koordinatensystems $(O,E_1,E_2,E_3)$.

> **Definition.** Ein affines Koordinatensystem heißt **kartesisch**, wenn die Koordinatenachsen paarweise orthogonal sind und die Längen der Einheitsstrecken übereinstimmen.[^2]

**Beispiel 2.** Das natürliche Koordinatensystem in $\mathbb{R}^3$ mit Ursprung und den Einheitspunkten
$$
  O(0,0,0)\,,\quad E_1(1,0,0)\,,\quad E_2(0,1,0)\,,\quad E_3(0,0,1)
$$
ist kartesisch, da die Einheitslängen gleich sind
$$
  \overline{OE_j}=(e_j)^2=1\quad\leftrightarrow\quad \overrightarrow{OE_j}\cdot\overrightarrow{OE_j}=1
$$
für beliebige $j\in\{1,2,3\}$ und die Koordinatenachsen paarweise orthogonal sind
$$
  OE_j\perp OE_k\quad\leftrightarrow\quad \overrightarrow{OE_j}\cdot\overrightarrow{OE_k}=0
$$
für beliebige $(j,k)\in\{1,2,3\}^2$ mit $j\not=k$.

**Beispiel 3.** Der Abstand zweier Punkte $X$ und $Y$ in der Ebene $\mathcal{A}^2$ berechnet sich bezüglich eines affinen Koordinatensystems $(O,E_1,E_2)$ mithilfe des Kosinus\-satzes in einem allgemeinen Dreieck
$$
  \overline{XY}^2=
  (y_1-x_1)^2\cdot (e_1)^2+(y_2-x_2)^2\cdot (e_2)^2 \\
  -2\cdot(y_1-x_1)\cdot(y_2-x_2)\cdot|\overrightarrow{OE_1}\cdot\overrightarrow{OE_2}|
$$
Ist das Koordinatensystem speziell kartesisch, so ergibt sich hieraus der Abstand mittels
$$
  \overline{XY}=\sqrt{(y_1-x_1)^2+(y_2-x_2)^2}
$$

Sicher gewusst?
===============

**Frage 1.** Nachstehend sind drei grundsätzlich zu unterscheidende Lagen von $X$ bezüglich des Paares $(O,E)$ angegeben. Ordnen Sie die Abbildungen den Größenangaben für
$$
  x=TV(X,E,O)
$$
zu.

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 415px;" -->
``````````````````````````````````````````````````
O     E                     O     E     O     E
*-----*-------*     *-------*-----*     *-----*
              X     X                   X
      (a)                 (b)             (c)
``````````````````````````````````````````````````

[[(a)] [(b)] [(c)]]
[( ) ( ) (X)]  $x=0$
[(X) ( ) ( )]  $x>0$
[( ) (X) ( )]  $x<0$


**Frage 2.** Das Teilverhältnis des Punktes $X$ bezüglich $(A,B)$ mit $$ A(1,0)\,,\quad B(3,0)\,,\quad X(-5,0)$$ berechnet sich

[( )] $TV(X,B,A)=-5$
[( )] $TV(X,B,A)=-\frac{1}{3}$
[(X)] $TV(X,B,A)=-3$

[^1]: Im zu $O\in\mathcal{A}^d$, $d\in\{2,3\}$, gehörigen Ortsvektorraum bilden die zu $E_1$ bis $E_d$ gehörenden Ortsvektoren eine Basis, worin sich der Ortsvektor eines jeden Punktes $X\in\mathcal{A}^d$ mit den Koeffizienten $(x_1,\ldots,x_d)$ eindeutig darstellen lässt.

[^2]: Diesen Einheitsstrecken wird die Länge $1$ zugewiesen.

[^3]: Soll eine Strecke durch einen Punkt der Strecke im Verhältnis $m:n$ geteilt werden, so existieren für $m\in\mathbb{N}$ und $n\in\mathbb{N}$ mit $m$ verschieden von $n$ stets zwei Möglichkeiten.

[^4]: Sofern nicht in Richtung der Geraden $g$ projiziert wird.


### Teilungspunkt einer Strecke

Zu einem gegebenem Teilverhältnis soll der Teilungspunkt einer Strecke unter Benutzung der Ortsvektoren der Punkte dargestellt werden.

Gegeben ist eine Verbindungsgerade $g=AB$ zweier Punkte $A$ und $B$ und ein weiterer Punkt $C\in g$. Für das Teilverhältnis von $C$ bezüglich $[A,B]$ gelte
$$
  \lambda=TV(C,B,A)=\epsilon\cdot\frac{\overline{AC}}{\overline{AB}}\quad\leftrightarrow\quad \overrightarrow{AC}=\lambda\cdot\overrightarrow{AB}
$$
Unter Benutzung der Ortsvektoren $a=\overrightarrow{OA}$, $b=\overrightarrow{OB}$ und $c=\overrightarrow{OC}$ stellt sich $c$ dar
$$
  c-a=\lambda\cdot(b-a)\quad\leftrightarrow\quad c=(1-\lambda)\cdot a+\lambda\cdot b
$$

Werden die Argumente in $TV(\,.\,)$ permutiert, so ändert sich im Allgemeinen der Wert des Teilverhältnisses.

> **Satz.** Für das Teilverhältnis eines Punktes $C\in g$ auf der Verbindungsgeraden $g=AB$ zweier verschiedener Punkte $A$ und $B$ gelte
> $$
  \lambda=TV(C,B,A) \in\mathbb{R}
$$
> Durch Permutation der Argumente nimmt das Teilverhältnis - soweit erklärt - einen der Werte
> $$ \lambda\,,\quad 1-\lambda\,,\quad \frac{1}{\lambda}\,,\quad \frac{1}{1-\lambda}\,,\quad 1-\frac{1}{\lambda}\,,\quad \frac{\lambda}{\lambda-1}
$$
> an. Insbesondere gelten:
>
> 1. Für  $C\not=A$ gilt: $\;TV({\color{blue}B},{\color{red}C},A)=TV({\color{red}C},{\color{blue}B},A)^{-1}=\frac{1}{\lambda}$
> 2. Es gilt: $\;TV(C,{\color{blue}A},{\color{red}B})=1-TV(C,{\color{red}B},{\color{blue}A})=1-\lambda$

**Beweis.** Zum Beweis der beiden letzten Aussagen kann ein Koeffizientenvergleich zwischen den Linearkombinationen von linker und rechter Seite angestrebt werden.

Für $\lambda\not=0$ ergibt sich schrittweise aus $\lambda=TV(C,B,A)$
$$
  c=(1-\lambda)\cdot a+\lambda\cdot b\quad\leftrightarrow\quad b=\frac{\lambda-1}{\lambda}\cdot a+\frac{1}{\lambda}\cdot c
$$
Andererseits gilt:
$$
  \mu=TV(B,C,A)\quad\leftrightarrow\quad b=(1-\mu)\cdot a+\mu\cdot c
$$
Aus dem Koeffizientenvergleich der rechten Seiten ergibt sich unmittelbar $\mu=\frac{1}{\lambda}$.

Des Weiteren gilt offensichtlich
$$
  \nu=TV(C,A,B)\quad\leftrightarrow\quad
  c=\nu\cdot a+(1-\nu)\cdot b\quad\leftrightarrow\quad
  \lambda=1-\nu
$$

Die beiden Permutationen
$$
  p=\begin{pmatrix} {\color{red}C} & {\color{blue}B} & A \\ {\color{blue}B} & {\color{red}C} & A \end{pmatrix}\,,\quad
  q=\begin{pmatrix} C & {\color{red}B} & {\color{blue}A} \\ C & {\color{blue}A} & {\color{red}B} \end{pmatrix}
$$
stellen eine Transposition (Tauschung) der ersten bzw. letzten beiden Elemente dar. Mit Hilfe der Hintereinanderausführung dieser lassen sich alle Permutationen von $(A,B,C)$ darstellen, woraus sich die genannten Werte des Teilverhältnisses ergeben.
$$
  \lambda\;\stackrel{p}{\longleftrightarrow}\;
  \frac{1}{\lambda}\;\stackrel{q}{\longleftrightarrow}\;
  1-\frac{1}{\lambda}\;\stackrel{p}{\longleftrightarrow}\;
  \frac{\lambda}{\lambda-1}\;\stackrel{q}{\longleftrightarrow}\;
  \frac{1}{1-\lambda}\;\stackrel{p}{\longleftrightarrow}\;
  1-\lambda\;\stackrel{q}{\longleftrightarrow}\;\lambda
$$

$\square$

**Beispiel 1.** Für den Mittelpunkt $M$ einer Strecke $[A,B]$ ergeben sich einsichtig die Teilverhältnisse

| $(\,.\,,\,.\,,\,.\,)$   | $TV(\,.\,)$   |
| :--------- | :--------- |
| $(M,B,A)$ | $\frac{1}{2}$ |
| $(B,M,A)$ | $2$           |
| $(B,A,M)$ | $-1$          |
| $(A,B,M)$ | $-1$          |
| $(A,M,B)$ | $2$           |
| $(M,A,B)$ | $\frac{1}{2}$ |

Interessant scheint, dass in diesem Beispiel von den sechs verschiedenen Teilverhältnis-Werten lediglich drei voneinander verschieden sind.

Für den Teilungspunkt $T$ einer Strecke $[P,Q]$ mit gegebenem Teilverhältnis $TV(P,Q,T)=\lambda$ ergibt sich unter Verwendung der Ortsvektoren
$$
  t=\frac{1}{1-\lambda}\cdot p-\frac{\lambda}{1-\lambda}\cdot q
$$
Teilt $T$ die Strecke $[P,Q]$ innen im Verhältnis $\alpha:\beta$, so ergeben sich $\lambda=-\frac{\alpha}{\beta}$ und somit die Koeffizienten
$$
  \frac{1}{1-\lambda}=\frac{\beta}{\alpha+\beta}\,,\quad
  -\frac{\lambda}{1-\lambda}=\frac{\alpha}{\alpha+\beta}
$$
Der innere Teilungspunkt $T$ auf $[P,Q]$ stellt sich somit dar
$$
  t=\frac{1}{\alpha+\beta}\cdot\left(\beta\cdot p+\alpha\cdot q\right)
$$
$T$ lässt sich physikalisch als [Schwerpunkt](#Massenschwerpunkt) des Massesystems $\{P,Q\}$ deuten, wenn
* $P$ das Gewicht $\beta$
* $Q$ das Gewicht $\alpha$
* und somit dem Massesystem $\{P,Q\}$ das Gesamtgewicht $\alpha+\beta$
zugewiesen wird.

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 415px;" -->
``````````````````````````````````````````````````
     P                        T            Q
     *------------------------*------------*
     |                       / \           |
     #                      .---.          #
     b                                     |
                                           #
                                           a
``````````````````````````````````````````````````


### Massenschwerpunkt

> **Satz.** Zu einem gegebenen System von Punkten $P_i$ mit den Ortsvektoren $p_i$ und den Gewichten $\gamma_i>0$ bestimmt
> $$
  s=\frac{1}{\sum_{i=1}^n{\gamma_i}}\cdot\sum_{j=1}^n{\left(\gamma_j\cdot p_j\right)}
$$
> den Massenschwerpunkt $S$ dieses Systems, worin $n\in\mathbb{N}$.

**Beweis.** Die Schwerpunktformel kann unter Benutzung des Verfahrens der vollständigen Induktion bewiesen werden.

1.*Induktionsanfang*. Die Formel ist für $n=1$ offensichtlich wahr, ebenso für $n=2$ nach dem voranstehenden Abschnitt.
2.*Induktionsschluss*. Die Schwerpunktformel gelte für $n=k$. Dann folgt für $n=k+1$
$$
  s_{k+1}=\frac{1}{{\color{red}\gamma_{k+1}+}\sum_{i=1}^k{\gamma_i}}\cdot
  \left({\color{red}\gamma_{k+1}\cdot p_{k+1}+}\sum_{j=1}^k{\left(\gamma_j\cdot p_j\right)}\right)=
  \frac{1}{\sum_{i=1}^{k+1}{\gamma_i}}\cdot\sum_{j=1}^{k+1}{\left(\gamma_j\cdot p_j\right)}
$$
$\square$

**Beispiel 1.** Zur Berechnung des Schwerpunktes dreier Massenpunkte
$$
  X(x,y)\,,\;\gamma_X>0\quad\longleftrightarrow\quad(x,y,\gamma_X)
$$
stellt sich die Formel wie folgt dar. Beachten Sie, dass im nachfolgend dargestellten Minimalcode keine Spezifikation der Variablen der Gewichte $\gamma$ vorgenommen worden ist.

``` javascript
A=[xA,yA,gammaA]
B=[xB,yB,gammaB]
C=[xC,yC,gammaC]
m=A[3]+B[3]+C[3]
1/m*(A[1]*A[3]+B[1]*B[3]+C[1]*C[3])
1/m*(A[2]*A[3]+B[2]*B[3]+C[2]*C[3])
```
@Algebrite.eval

**Beispiel 2.** Der Schwerpunkt der Ecken eines Dreiecks, die jeweils mit *gleichen Massen* belegt sind, ist der gemeinsame Punkt der drei (!) Seitenhalbierenden.

![Schwerpunkt](img/geo-bild03.png "_Fig._ Schwerpunkt $S$ eines Dreiecks $ABC$ bei gleicher Masseverteilung in den Ecken.")

Für den Nachweis wird ein affines Koordinatensystem mit Ursprung $O=A$ und den Einheitspunkten $E_1=B$ sowie $E_2=C$ gewählt.

Die Halbierungspunkte der Seiten des Dreiecks berechnen sich in diesem Koordinatensystem einsichtig zu
$$
  H_{AB}\left(\frac{1}{2},0\right)\,,\quad
  H_{AC}\left(0,\frac{1}{2}\right)\,,\quad
  H_{BC}\left(\frac{1}{2},\frac{1}{2}\right)
$$
die Seitenhalbierenden besitzen die Gleichungen
$$
  H_{AB}C\,:\;\;2\cdot x_1+x_2=1\,,\quad
  H_{BC}A\,:\;\;x_1-x_2=0\,,\quad
  H_{AC}B\,:\;\;x_1+2\cdot x_2=1
$$
Der gemeinsame Punkt der drei Geraden ergibt sich als Lösung eines Systems linearer Gleichungen
$$
  \begin{pmatrix} 2 & 1 & 1 \\ 1 & -1 & 0 \\ 1 & 2 & 1 \end{pmatrix}\;\stackrel{Z_3+Z_2\to Z_3}{\longrightarrow}\;\begin{pmatrix} 2 & 1 & 1 \\ 1 & -1 & 0 \\ 2 & 1 & 1 \end{pmatrix}\;\rightarrow\;\begin{pmatrix} 1 & -1 & 0 \\ 2 & 1 & 1 \end{pmatrix}\;\stackrel{Z_2-2\cdot Z_1\to Z_2}{\longrightarrow}\;\begin{pmatrix} 1 & -1 & 0 \\ 0 & 3 & 1 \end{pmatrix}\;\stackrel{Z_2\cdot\frac{1}{3}\to Z_2}{\longrightarrow}\;\begin{pmatrix} 1 & -1 & 0 \\ 0 & 1 & \frac{1}{3} \end{pmatrix}
$$
Die eindeutig bestimmten Koordinaten von $S$ sind
$$
  S\left(\frac{1}{3},\frac{1}{3}\right)
$$
Ein Vergleich mit der Schwerpunktformel unter Benutzung der Ortsvektoren der Eckpunkte ergibt
$$
  s=\frac{1}{3}\cdot a+\frac{1}{3}\cdot b+\frac{1}{3}\cdot c=\frac{1}{3}\cdot\begin{pmatrix} 1 \\ 1\end{pmatrix}
$$
Des Weiteren folgt aus der Konstruktion des Schwerpunktes $S$ des Dreiecks $ABC$, dass $S$ die Schwerlinien
$$
  [A,H_{BC}]\,,\quad [B,H_{AC}]\,,\quad [C,H_{AB}]
$$
im Verhältnis $2:1$ der Teillängen teilt, d. h.
$$
  TV(S,H_{BC},A)=TV(S,H_{AC},B)=TV(S,H_{AB},C)=\frac{2}{3}
$$


### Konvexe Punktmengen

Bei der Berechnung des Schwerpunktes eines Massensystems ist zu beobachten, dass für die Koeffizienten der Linearkombinationen
$$
 \alpha_j:=\frac{\gamma_j}{\sum_{i=1}^n{\gamma_i}}
$$
stets die Bedingungen
$$
  \alpha_j\geq0\;\;\forall j\in\{1,2,\ldots,n\}\quad\text{sowie}\quad \sum_{j=1}^n{\alpha_j}=1
$$
gelten.

> **Definition.** Eine Linearkombination der Form
>$$
  x=\alpha_1\cdot x_1+\alpha_2\cdot x_2+\ldots+\alpha_n\cdot x_n $$
>mit Vektoren $x_j$ und Skalaren $\alpha_j$, für die gelten
>$$
  \alpha_j\geq0\;\;\forall j\in J\quad\text{sowie}\quad \sum_{j\in J}{\alpha_j}=1 $$
> heißt eine **Konvexkombination** der Vektoren $x_j$.

**Beispiel 1.** Für zwei verschiedene Punkte $X_1$ und $X_2$ beschreibt die Konvexkombination in den Ortsvektoren der Punkte
$$
  x=\alpha_1\cdot x_1+\alpha_2\cdot x_2\,,\quad \alpha_1\geq0\wedge\alpha_2\geq0\,,\quad \alpha_1+\alpha_2=1
$$  
das Innere (und die Randpunkte) der Strecke $[X_1,X_2]$. Mit der Bezeichnung $\alpha=\alpha_2$ und $\alpha_1=1-\alpha$ folgt
$$
  x=(1-\alpha)\cdot x_1+\alpha\cdot x_2
$$
d. h. die Beschreibung eines Teilungspunktes auf der Strecke $[X_1,X_2]$.

**Beispiel 2.** Für drei verschiedene, nicht kollineare Punkte $X_1$, $X_2$ und $X_3$ beschreibt die Konvexkombination in den Ortsvektoren der Punkte
$$
  x=\alpha_1\cdot x_1+\alpha_2\cdot x_2+\alpha_3\cdot x_3\,,\quad \alpha_1\geq0\wedge\alpha_2\geq0\wedge\alpha_3\geq0\,,\quad \alpha_1+\alpha_2+\alpha_3=1
$$  
das Innere (und die Seiten) des Dreiecks $X_1X_2X_3$. Mit der Bezeichnung $\alpha=\alpha_2$, $\beta=\alpha_3$ und $\alpha_1=1-\alpha-\beta$ folgt
$$
  x=(1-\alpha-\beta)\cdot x_1+\alpha\cdot x_2+\beta\cdot x_3=x_1+\alpha\cdot(x_2-x_1)+\beta\cdot(x_3-x_1)
$$
d. h. ein Punkt auf der durch $X_1$, $X_2$ und $X_3$ bestimmten Dreiecksfläche.

**Beispiel 3.** Entsprechend ergibt sich das Innere eines Tetraeders als Konvexkombination seiner Eckpunkte.

**Bemerkung.** Allgemein heißt eine Punktmenge $M$ **konvex**, wenn mit zwei beliebigen Punkten $P\in M$ und $Q\in M$ auch alle Punkte der Strecke $[P,Q]$ in $M$ oder auf dem Rand von $M$ liegen. Siehe [Konvexe Mengen](https://de.wikipedia.org/wiki/Konvexe_Menge).

Eine Erklärung der Konvexität von Mengen finden Sie alternativ in folgendem Video.

!?[youtube](https://www.youtube.com/watch?v=C1xN4KpySn0)

In der folgenden Abbildung sind ein konvexes Sechseck (links) und ein nicht-konvexes Sechseck abgebildet.

<!-- style="display: block; margin-left: auto; margin-right: auto; max-width: 415px;" -->
``````````````````````````````````````````````````
   A  *---------*  B             A  *---------*  B
     /           \                   \       /
    /             \                   \     /
F  *               *  C             F  *   *  C
    \             /                   /     \
     \           /                   /       \
   E  *---------* D              E  *---------* D
``````````````````````````````````````````````````

Der Durchschnitt aller konvexen Mengen, die eine gegebene Punktmenge $\{X_1,X_2,\ldots,X_n\}$ enthalten, wird **konvexe Hülle** von $X_1$, $X_2$ bis $X_n$ genannt. Analytisch wird diese durch die Menge aller Konvexkombinationen der Punkte $X_j$ bestimmt.

Ein effizienter Algorithmus zur Berechnung der konvexen Hülle ist [Graham Scan](https://de.wikipedia.org/wiki/Graham_Scan).


Sicher gewusst?
===============

**Frage.** Gegeben sind die folgenden Punktmengen und das dargestellte Polygon (rot).

![wikipedia](img/Convex_hull.png)<!-- style="width: 75%;" -->

Entscheiden Sie, welche der Punktmengen das rot umrandete Gebiet als konvexe Hülle besitzt.

[[ ]] $\{(x,y)\; (0\leq x\leq4\;\wedge\;0\leq y\leq2)\}$
[[X]] $(0,0)$, $(0,1)$, $(1,2)$, $(2,2)$, $(1,1)$, $(2,1)$, $(3,1)$ und $(4,0)$
[[ ]] $(0,0)$, $(0,1)$, $(1,2)$, $(3,1)$ und $(4,0)$
[[X]] $(0,0)$, $(0,1)$, $(1,2)$, $(2,2)$ und $(4,0)$
[[?]] Stellen Sie die Punktmengen in einem kartesischen Koordinatensystem graphisch dar.
****************************************

Die Punktmenge $$ \{(x,y)\; (0\leq x\leq4\;\wedge\;0\leq y\leq2)\} $$ bildet ein achsenparalleles Rechteck mit den Seitenlängen vier und zwei: Es ist eine konvexe Menge, jedoch nicht die dargestellte. Die Punktmenge $(0,0)$, $(0,1)$, $(1,2)$, $(3,1)$ und $(4,0)$ enhält nicht den Eckpunkt $(2,2)$ des dargestellten Polygons. Die beiden anderen Punktmengen besitzen das rot umrandete Gebiet als konvexe Hülle.

****************************************


### De Casteljau Algorithmus

Unter Verwendung des Teilverhältnisses auf Strecken lassen sich ebenso Kurven konstruieren. In diesem Abschnitt wird das Konzept einer **Bézierkurve** vorgestellt, die sich interaktiv und koordinatenunabhängig über ihr Kontrollpolygon konstruieren lässt. Siehe [Bézierkurve](https://www.geogebra.org/m/Ger7Xvut#material/PvCmYWMZ "de casteljau Algorithmus für Bézierkurve ").

Das Konzept der Bézierkurven wurde unabhängig voneinander von Pierre Bézier bei Renault (1959) und von Paul de Casteljau bei Citroën (1962) für die computerunterstützte Konstruktion entwickelt.

**Beispiel 1.** Gegeben ist ein aus zwei Strecken bestehendes Polygon mit den Eckpunkten $P_0$, $P_1$ und $P_2$. Diese werden Kontrollpunkte genannt und bilden die Eckpunkte eines Kontrollpolygons $P_0-P_1-P_2$.

![Bézierkurve](img/geo-bild04.png "_Fig._ Bézierkurve vom Grad $2$ mit Konstrollpolygon $P_0-P_1-P_2$.")

Verfahren für $n=2$
===================

1. Stufe
--------

Bilde auf den Seiten des Kontrollpolygons den Teilungspunkt $P_i^1$ zum gleichen Wert $t\in[0,1]$ des Teilverhältnisses. Unter Verwendung der Ortsvektoren der Kontrollpunkte stellen sich die Teilungspunkte dar $$
  p_0^1=(1-t)\cdot p_0+t\cdot p_1\,,\quad
  p_1^1=(1-t)\cdot p_1+t\cdot p_2
$$ Diese bilden Teilungspunkte $1$-ter Stufe.

2. Stufe
--------

Die Teilungspunkte $P_0^1$ und $P_1^1$ auf den Seiten des Kontrollpolygons bilden eine neue Strecke. Konstruiere auf dieser den Teilungspunkt $P_0^2$ zweiter Stufe zum gleichen Wert $t\in[0,1]$ des Teilverhältnisses. $$
  p_0^2=(1-t)\cdot p_0^1+t\cdot p_1^1
$$ Das Verfahren endet hier.[^1]

Variation des Parameters
------------------------

Wird der Parameter $t\in[0,1]$ variiert, so entstehen verschiedene Lagen des Punktes $P_0^2(t)$, die als Linearkombination der Ortsvektoren der Kontrollpunkte dargestellt werden können $$\begin{array}{rl}
  x(t)=p_0^2(t) & =(1-t)\cdot\left((1-t)\cdot p_0+t\cdot p_1\right)+t\cdot\left((1-t)\cdot p_1+t\cdot p_2\right) \\
  & = (1-t)^2\cdot p_0+2\cdot t\cdot (1-t)\cdot p_1+t^2\cdot p_2
\end{array}$$ Im Beispiel entspricht dies einer quadratischen Parametrisierung der Menge aller Punkte $P_0^2(t)$, die **Bézierkurve** $c$ der Ordnung $n=2$ zum Kontrollpolygon $P_0-P_1-P_2$ genannt wird.

![geogebra](img/geo-animation-1.png "*Fig.* Konstruktion einer Bézierkurve zu ihrem Kontrollpolygon $P_0-P_1-P_2$ nach dem de Casteljau Algorithmus.")

**Bemerkung 1.** Die Koeffizienten der angegebenen Linearkombination sind Polynome zweiten Grades $$
  B_0^2(t)=(1-t)^2\,,\quad B_1^2(t)=2\cdot t\cdot (1-t)\,,\quad B_2^2(t)=t^2
$$ und werden Bernsteinpolynome (zweiten Grades) genannt. Für diese gelten $$
  B_0^2(t)\geq0\;\wedge\; B_2^2(t)\geq0\;\wedge\;\left(B_1^2(t)\geq0\;\leftrightarrow\; t\in[0,1]\right)
$$ sowie $$
  \sum_{i=0}^ 3{B_i^2(t)}=(1-t)^2+2\cdot t\cdot (1-t)+t^2=1
$$ Hieraus folgt, dass die auf dem Intervall $[0,1]\ni t$ definierte Bézierkurve $c$ in der konvexen Hülle ihrer Kontrollpunkte liegt.

Die [Bernsteinpolynome](https://de.wikipedia.org/wiki/Bézierkurve) lassen sich allgemein berechnen mit Hilfe der Formel $$
  B_i^n(t)=\binom{n}{i}\cdot(1-t)^{n-i}\cdot t^i\,,\quad n\in\mathbb{N}\,,i\in\mathbb{N}\,,i\leq n
$$ worin für die Bernsteinpolynome zweiten Grades $n=2$ zu setzen ist.

```javascript
B2=binomial(2,m)*t^m*(1-t)^(2-m)
B02=subst(0,m,B2)
B12=subst(1,m,B2)
B22=subst(2,m,B2)
B02
B12
B22
simplify(B02+B12+B22)
```
@Algebrite.eval

**Bemerkung 2.** Für den Tangentenvektor an eine Bézierkurve in $X(t_0)\in c$ mit $t_0\in[0,1]$ gilt $$ \begin{array}{rl}
  \dot{x}(t_0) & =\frac{\mathrm{d}}{\mathrm{d}t}{x(t)}|_{t=t_0}=\dot{B}_0^2(t_0)\cdot p_0+\dot{B}_1^2(t_0)\cdot p_1+\dot{B}_2^2(t_0)\cdot p_2 \\
  & = -2\cdot(1-t)\cdot p_0+\left(2\cdot(1-t)-2\cdot t\right)\cdot p_1+2\cdot t\cdot p_2 \\
  & = 2\cdot\left(p_1^1-p_0^1\right)(t_0)
  \end{array}
$$ d. h. die Trägergerade der Strecke $\left[P_0^1,P_1^1\right]$ aus der vorletzten Stufe des de Casteljau Algorithmus bildet die Tangente an $c$ im Kurvenpunkt $X(t_0)$. Insbesondere bilden $$
  2\cdot(p_1-p_0) \quad\text{bzw.}\quad 2\cdot(p_2-p_1)
$$ die Tangentenvektoren in $X(0)=P_0$ beziehungsweise $X(1)=P_2$.

```javascript
d(B02)
d(B12)
d(B22)
x=d(B02*p0+B12*p1+B22*p2)
p01=(1-t)*p0+t*p1
p11=(1-t)*p1+t*p2
x-2*(p11-p01)
```
@Algebrite.eval


Verfahren für allgemeines $n\in\mathbb{N}$
==========================================

Das Verfahren zur Konstruktion einer Bézierkurve zweiter Ordnung kann auf den Fall $n>2$ verallgemeinert werden. Hierfür wird das Kontrollpolygon $$ P_0-P_1-...-P_{n-1}-P_n $$ vorausgesetzt. Das nachfolgende Schema zeigt das wiederholte Bestimmen der Teilungspunkte $P_j^k$ $k$-ter Stufe auf den Polygonen $(k-1)$-ter Stufe ($k\in\mathbb{N},1\leq k\leq n$), wobei $k-1=0$ für das Kontrollpolygon gesetzt wird.

<!-- style="display: block; margin-left: auto; margin-right: auto;" -->
![Bézierkurve2](img/geo-bild05.png "_Fig._ Schema zur Bildung der Teilungspunkte nach dem de Casteljau Algorithmus.")

Der Teilungspunkt $X(t)=P_0^{n}(t)$ ist Kurvenpunkt der **Bézierkurve** $n$-ter Ordnung zum Kontrollpolygon $P_0-P_1-...-P_{n-1}-P_n$ und besitzt die Parametrisierung $$
  x(t)=\sum_{i=0}^n{B_i^n(t)\cdot p_i}\,,\quad n\in\mathbb{N},\;n\geq2
$$ worin $p_i$ die Ortsvektoren der Kontrollpunkte $P_i$ und $$
  B_i^n(t)=\binom{n}{i}\cdot(1-t)^{n-i}\cdot t^i
$$ die **Bernsteinpolynome** $n$-ten Grades bezeichnen.[^2]

Die Konstruktion einer Bézierkurve vom Grad $n=3$ nach dem de Casteljau Algorithmus finden Sie im folgenden Video.

!?[youtube](https://www.youtube.com/watch?v=YATikPP2q70)

**Bemerkung 3.** *Exkurs Mathematik.* Die in der Darstellung der Bernsteinpolynome auftretenden [Binomialkoeffizienten](https://de.wikipedia.org/wiki/Binomialkoeffizient) lassen sich definieren als $$
  \binom{n}{i}=\frac{n!}{i!\cdot(n-i)!}\,,\quad n\in\mathbb{N}\,,\;i\in\mathbb{N}\,,\; i\leq n
$$ sowie $$
  \binom{n}{i}=0\,,\quad n\in\mathbb{N}\,,\;i\not\in\{0,1,...,n\}
$$ Sie beschreiben die Anzahl aller Möglichkeiten, aus einer Menge von $n$ verschiedenen Elementen genau $i$ ohne Zurücklegen und ohne Berücksichtigung der Reihenfolge auszuwählen. Für $n=2$ ergeben sich im Speziellen $$
  \binom{2}{0}=1\,,\quad\binom{2}{1}=2\,,\quad\binom{2}{2}=1
$$ die beispielsweise als Koeffizienten in der binomischen Formel $$
  (a+b)^2=1\cdot a^2+2\cdot a\cdot b+1\cdot b^2
$$ auftreten. Warum?

>**Satz 1.** *Refursionsformel.* Die Bernsteinpolynome $n$-ten Grades lassen sich rekursiv definieren vermöge $$
  B_i^n(t)=(1-t)\cdot B_i^{n-1}(t)+t\cdot B_{i-1}^{n-1}(t)\quad \forall \,t\in\mathbb{R}
$$ d. h. als Linearkombination von Bernsteinpolynomen $(n-1)$-ten Grades.

**Beweisidee.** Der Nachweis der Rekursionsformel kann durch direktes Nachrechnen unter Benutzung der Definition der Bernsteinpolynome geführt werden.

**Beispiel 2.** Die Bernsteinpolynome für $n\leq2$ sollen rekursiv berechnet und mit der expliziten Bildungsvorschrift vergleichen werden. Für $n=0$ ergeben sich $$
  B_0^0(t)=1\,,\quad B_i^0=0 \quad\forall\, i\not\in\{0,1,...,n\}
$$ Aus diesen lassen sich alle Bernsteinpolynome für den Indexwert $n=1$ berechnen $$
  B_0^1(t)=(1-t)\cdot B_0^0(t)+t\cdot B_{-1}^0(t)=1-t\,,\quad B_1^1(t)=(1-t)\cdot B_1^0(t)+t\cdot B_{0}^0(t)=t
$$ Unter erneuter Benutzung der Rekursionsformel ergeben sich die Bernsteinpolynome für den Indexwert $n=2$: $$
  \begin{array}{rl}
    B_0^2(t) & =(1-t)\cdot B_0^1(t)+t\cdot B_{-1}^1(t)=(1-t)^2 \\
    B_1^2(t) & =(1-t)\cdot B_1^1(t)+t\cdot B_{0}^1(t)=2\cdot(1-t)\cdot t \\
    B_2^2(t) & =(1-t)\cdot B_2^1(t)+t\cdot B_{1}^1(t)=t^2
  \end{array}
$$

**Bemerkung 4.** Siehe [Pascalsches Dreieck](https://de.wikipedia.org/wiki/Pascalsches_Dreieck).

>**Satz 2.** Für die Bernsteinpolynome $n$-ten Grades gilt $$
  \sum_{i=0}^n{B_i^n(t)}=1\quad \forall \,t\in\mathbb{R}
$$

**Beweis.** Die Gültigkeit der Aussage folgt aus der Gleichung $$
  1=1^n=(t+(1-t))^n=\sum_{i=0}^n{\binom{n}{i}\cdot t^i\cdot (1-t)^{n-i}}=\sum_{i=0}^n{B_i^n(t)}
$$ unter Benutzung des [Binomischen Lehrsatzes](https://de.wikipedia.org/wiki/Binomischer_Lehrsatz). $\square$

```javascript
n=3
sum(binomial(n,i)*t^i*(1-t)^(n-i),i,0,n)
```
@Algebrite.eval

>**Satz 3.** Eine Bézierkurve mit der Parametrisierung $$
  t\mapsto x(t)=\sum_{i=0}^n{B_i^n(t)\cdot p_i}\,,\quad n\in\mathbb{N},\;n\geq2
$$ ist für jede Wahl $t\in[0,1]$ eine Konvexkombination der Kontrollpunkte, d. h. die Kurve liegt in der konvexen Hülle ihrer Kontrollpunkte.

**Beweis.** Diese Aussage folgt direkt aus Satz 2 und der Aussage, dass für alle Bernsteinpolynome $B_i^n(t)\geq0$ gilt, falls $t\in[0,1]$. Dies lässt sich unmittelbar mittels vollständiger Induktion aus der Rekursionsformel ableiten (Satz 1). $\square$

>**Satz 4.** Eine Bézierkurve mit der Parametrisierung $$
  t\mapsto x(t)=\sum_{i=0}^n{B_i^n(t)\cdot p_i}\,,\quad n\in\mathbb{N},\;n\geq2
$$ und $t\in[0,1]$ ist Parameterdarstellung der Bézierkurve nach dem de Casteljau Algorithmus.

Auf einen Nachweis dieser Aussage wird hier verzichtet.

Auf weitere Eigenschaften von Bézierkurven sowie deren Verwendung im Kurvendesign wird später genauer eingegangen.


Sicher gewusst?
===============

**Frage 1.** Für die Binomialkoeffizienten $$
  \binom{n}{i} \quad\text{mit}\quad n\in\mathbb{N}\,,\;i\in\mathbb{N}\,,\;i\leq n
$$ gilt die Symmetrie
$$
  \binom{n}{i}=\binom{n}{n-i}
$$

[[X]] Wahr
[[ ]] Falsch
[[?]] Die Binomialkoeffizienten berechnen sich für $n\in\mathbb{N}$ und $i\in\mathbb{N}$ mit $i\leq n$ nach der Formel $$
  \binom{n}{i}=\frac{n!}{i!\cdot(n-i)!}
$$
****************************************

Der Binomialkoeffizient berechnet sich $$
  \binom{n}{n-i}=\frac{n!}{(n-i)!\cdot(n-(n-i))!}=\frac{n!}{(n-i)!\cdot i!}=\binom{n}{i} $$

****************************************

**Frage 2.** Gegeben ist das Kontrollpolygon $$
  P_0(1,0)\,,\quad P_1(1,1)\,,\quad P_2(0,1)
$$ einer Bézierkurve $c$. Kennzeichnen Sie die wahren Aussagen in den folgenden Aussagen.

[[ ]] $c$ enthält den Punkt $P_1$.
[[X]] $c$ berührt die Geraden $y=1$ und $x=1$.
[[ ]] $c$ berührt die Geraden $y=0$ und $x=0$.
[[X]] $c$ enthält den Punkt $P_0$.
[[X]] $c$ liegt im Inneren und dem Rand des Dreiecks $P_0P_1P_2$.
[[?]] Die Bézierkurve $c$ besitzt die Parametrisierung in Bézierdarstellung $$ x(t)=\sum_{i=0}^n{B_i^2(t)\cdot p_i}\,,\quad t\in[0,1] $$ worin $B_i^2(t)$ die Bernsteinpolynome zweiten Grades und $p_i$ die Ortsvektoren der Kontrollpunkte bezeichnen.
****************************************

Die Bézierkurve $c$ interpoliert den ersten und letzten Punkt des Kontrollpolygons, also $P_0$ und $P_2$. $P_1\not\in c$, da $$ p_1\not=x(t)=(1-t)^2\cdot p_0+2\cdot(1-t)\cdot t\cdot p_1+t^2\cdot p_2\quad\forall\, t\in[0,1] $$ was nach Einsetzen der Ortsvektoren für $p_i$ ersichtlich ist. $c$ besitzt in $P_0$ den Tangentenvektor $2\cdot(p_1-p_0)$ und in $P_2$ den Tangentenvektor $2\cdot(p_2-p_1)$. Die Bézierkurve $c$ liegt in der konvexen Hülle der Kontrollpunkte.

****************************************

[^1]: Wird das Verfahren der Streckenteilung auf ein Kontrollpolygon mit $4$ Kontrollpunkten durchgeführt, so entstehen in der $1$-ten Stufe drei Teilungspunkte. Diese bilden ein Polygon $P_0^1-P_1^1-P_2^1$, auf welches der Teilungsschritt 2 wiederholt angewendet werden kann. Das Verfahren endet mit dem (einzigen) Teilungspunkt $P_0^3$.

[^2]: Genauer heißt $x(t)$ Bézierdarstellung, da diese Parametrisierung in der Basis der Bernsteinpolynome (hier zunächst ohne Nachweis) angegeben ist.


## Affine Transformationen

In diesem Kapitel werden folgende Themen behandelt:

* Affine Abbildungen, darunter Affinitäten und deren Eigenschaften
* Basis- und Koordinatentransformationen
* Spezielle Affinitäten
* Kongruenzen und Ähnlichkeiten
* Abbildungsgeometrische Erzeugung geometrischer Figuren


### Definition und allgemeine Eigenschaften

Definition affiner Abbildungen
==============================

>**Definition.** Eine Abbildung $\alpha:\mathcal{A}^d\to \mathcal{A}^d$ des $d$-dimensionalen affinen Raumes ($d\in\mathbb{N},\,d\geq1$), die Punkte $X$ auf Punkte $X^\prime=\alpha(X)$ abbildet, heißt **affine Abbildung**, wenn sie bezogen auf ein affines Koordinatensystem $[O,E_1,...,E_d]$ mit Ortsvektor $x=\overrightarrow{OX}$ und $x^\prime=\overrightarrow{OX^\prime}$ in der Form $$ x\mapsto x^\prime=A\cdot x+a $$ mit $A\in\mathbb{R}^{d,d}$ und $a\in\mathbb{R}^{d}$ beschrieben werden kann.

Die reelle, quadratische Matrix $A$ wird Abbildungsmatrix (Transformationsmatrix), der reelle Vektor $a$ Translationsvektor von $\alpha$ genannt. Für $d=2$ heißt $\alpha$ affine Abbildung der Ebene, für $d=3$ affine Abbildung des dreidimensionalen Raumes.

Die Abbildung von Punkten der Ebene beziehungsweise des dreidimensionalen Raumes wird in nachstehendem Video an Beispielen erläutert. (Der Translationsvektor ist in diesen Beispielen jeweils der Nullvektor.)

!?[Affine Abbildung](https://www.youtube.com/watch?v=r0CNAI2rHfY)

**Beispiel 1.** Die affine Abbildung $\alpha$ mit $$
  A=\mathrm{diag}{(1,1)}\,,\quad a=(2,-3)^\top
$$ worin $A$ die zweireihige Einheitsmatrix ist, beschreibt eine Translation aller Punkte $X$ der Ebene $\mathcal{A}^2$ mit dem Translationsvektor $a$.

```javascript
A=[[1,0],[0,1]]
a=[[2],[-3]]
x=[[x1],[x2]]
dot(A,x)+a
```
@Algebrite.eval

**Beispiel 2.** Die affine Abbildung $\alpha$ mit $$
  A=\mathrm{diag}{(1,1,0)}\,,\quad a=(0,0,0)^\top
$$ beschreibt die Projektion aller Punkte $X$ des dreidimensionalen Raumes $\mathcal{A}^3$ in die Ebene $z=0$ (*"Grundrissprojektion"*).

```javascript
A=[[1,0,0],[0,1,0],[0,0,0]]
a=[[0],[0],[0]]
x=[[x1],[x2],[x3]]
dot(A[1],x)+a[1]
dot(A[2],x)+a[2]
dot(A[3],x)+a[3]
```
@Algebrite.eval

>**Definition.** Eine affine Abbildung $$ x\mapsto x^\prime=A\cdot x+a $$ heißt **Affinität**, falls $A$ eine reguläre Matrix ist, d. h. falls $\det{A}\not=0$.

Das Beispiel 1 beschreibt eine Affinität, da $\det{A}=1$, Beispiel 2 hingegen eine affine Abbildung, die keine Affinität ist: Hier ist $\det{A}=0$.


Folgerungen
===========

Die nachstehenden Eigenschaften werden für den Fall $d=3$ formuliert, können jedoch für jede Wahl $d\geq 2$ in gleicher Weise angegeben werden.


Eigenschaft 1
-------------

Die Koordinaten des Bildpunktes $X'$ unter einer affinen Abbildung $\alpha$ berechnen sich linear in den Koordinaten des Urbildes $X$.

```javascript
A=[[a11,a12,a13],[a21,a22,a23],[a31,a32,a33]]
a=[[a1],[a2],[a3]]
x=[[x1],[x2],[x3]]
i=1
inner(A[i],x)+a[i]
```
@Algebrite.eval

In Matrixdarstellung stellt sich der Ortsvektor des Bildpunktes $X'$ für den Fall $d=3$ dar mittels
$$
  \begin{pmatrix} x_1' \\ x_2' \\ x_3' \end{pmatrix}=
  \begin{pmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{pmatrix}\begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}
$$
worin die Komponenten der Transformationsmatrix $A$ mit $a_{ij}$, die Komponenten des Translationsvektors $a$ mit $a_i$ bezeichnet sind.


Eigenschaft 2
-------------

Der Translationsvektor $a$ ist Ortsvektor des Bildpunktes $O'=\alpha(O)$ (Bild des Koordinatenursprungs), denn
$$
  \begin{pmatrix} o_1 \\ o_2 \\ o_3 \end{pmatrix}=
  \begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}\mapsto
  \begin{pmatrix} o_1' \\ o_2' \\ o_3' \end{pmatrix}=
  \begin{pmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{pmatrix}\begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}=\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}
$$


Eigenschaft 3
-------------

Für die Bilder der Einheitspunkte $E_i$, $i\in\{1,2,3\}$, mit den Ortsvektoren $e_i$ gelten
$$
  \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}\mapsto
  \begin{pmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{pmatrix}\begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}=\textcolor{magenta}{\begin{pmatrix} a_{11} \\ a_{21} \\ a_{31} \end{pmatrix}}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}
$$
und
$$
  \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix}\mapsto
  \begin{pmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{pmatrix}\begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}=\textcolor{magenta}{\begin{pmatrix} a_{12} \\ a_{22} \\ a_{32} \end{pmatrix}}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}
$$
sowie
$$
  \begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix}\mapsto
  \begin{pmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{pmatrix}\begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}=\textcolor{magenta}{\begin{pmatrix} a_{13} \\ a_{23} \\ a_{33} \end{pmatrix}}+\begin{pmatrix} a_1 \\ a_2 \\ a_3 \end{pmatrix}
$$
Das ist jeweils die Summe aus dem $i$-ten Spaltenvektor von $A$ und dem Translationsvektor $a$. Es gilt mit Eigenschaft 2
$$
  \overrightarrow{\alpha(O)\alpha(E_i)}=e_i'-o'=s_i\,,\quad i\in\{1,2,3\}
$$

Der $i$-te Spaltenvektor $s_i$ in der Spaltenvektordarstellung von $A$ $$ A=\begin{pmatrix} s_1 & s_2 & s_3 \end{pmatrix} $$ ist der Richtungsvektor $\overrightarrow{O'E_i'}$, der von $O'=\alpha(O)$ nach $E_i'=\alpha(E_i)$ zielt, dargestellt im affinen Koordinatensystem $(O,E_1,E_2,E_3)$.

![test](img/geo-bild06.png "*Fig.* Affines Koordinatensystem und dessen Bild unter der affinen Abbildung (mit oberem Index gekennzeichnet).")

<!-- style="color:magenta" -->**Merkregel.** "Willst die Matrix du erhalten, schreib die Bilder der Basis in die Spalten."

**Bemerkung.** Mit den Eigenschaften 2 und 3 haben Transformationsmatrix $A$ und Translationsvektor $a$ einer affinen Abbildung $\alpha$ geometrische Bedeutung. (Definition)


Eigenschaft 4
-------------

Für einen Punkt $X\in\mathcal{A}^3$ in allgemeiner Lage mit Ortsvektor / Koordinaten
$$
  x=\begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix}=
  \sum_{i=1}^3{x_i\cdot e_i}
$$
bezüglich der Basis $(e_1,e_2,e_3)$ ergibt sich der Bildvektor unter $\alpha$ gemäß
$$
  x'=\begin{pmatrix} x_1' \\ x_2' \\ x_3' \end{pmatrix}=
  A\cdot\left(\sum_{i=1}^3{x_i\cdot e_i}\right)+a=
  \left(\sum_{i=1}^3{x_i\cdot A\cdot e_i}\right)+a=
  \left(\sum_{i=1}^3{x_i\cdot s_i}\right)+a
$$
Aus dieser Darstellung lässt sich erkennen, dass $X$ im affinen Koordinatensystem $(O,E_1,E_2,E_3)$ dieselben Koordinaten besitzt wie Koeffizienten in der vorstehenden Linearkombination.

Ist die Transfomationsmatrix $A$ regulär[^1], so sind deren Spaltenvektoren linear unabhängig, so dass $\left(O',E_1',E_2',E_3'\right)$ wieder ein affines Koordinatensystem ist. Siehe Abschnitt [Affine Koordinatensysteme](#Affine-Koordinatensysteme). Hieraus ergibt sich folgende Erzeugungsmöglichkeit einer Affinität.

> **Erzeugung einer Affinität.** Sind zwei affine Koordinatensysteme $$
  \left(O,E_1,E_2,E_3\right)\,,\quad \left(O',E_1',E_2',E_3'\right)
$$ gegeben, so lässt sich eine Affinität $$
  \alpha:X\mapsto X'=\alpha(X)
$$ durch gleiche Koordinaten angeben: Das Urbild $X$ besitzt den Ortsvektor $$
  x=\begin{pmatrix} \blue{x_1} \\ \blue{x_2} \\ \blue{x_3} \end{pmatrix} \quad\text{bezüglich}\quad \left(O,E_1,E_2,E_3\right)
$$ und das Bild $X'=\alpha(X)$ besitzt den gleichen Ortsvektor $$
  x'=\begin{pmatrix} \blue{x_1} \\ \blue{x_2} \\ \blue{x_3} \end{pmatrix} \quad\text{bezüglich}\quad \left(O',E_1',E_2',E_3'\right)
$$


Eigenschaft 5
-------------

Der Differenzvektor zweier Punkte $X$ und $Y$ mit den Ortsvektoren $x$ beziehungsweise $y$
$$
  \overrightarrow{XY}=y-x=:z
$$
wird unter der affinen Abbildung $\alpha$ abgebildet auf
$$
  \overrightarrow{\alpha(X)\alpha(Y)}=y'-x'=(A\cdot y+a)-(A\cdot x+a)=A\cdot(y-x)=A\cdot z
$$
worin $x'$ beziehungsweise $y'$ die Ortsvektoren der Bilder $X'=\alpha(X)$ beziehungsweise $Y'=\alpha(Y)$ bezeichnen.

>**Definition.** Ist der Translationsvektor $a$ einer affinen Abbildung $\alpha$ der Nullvektor, d. h. $$
  \alpha:X\mapsto X'\quad\text{mit}\quad x\mapsto x'=A\cdot x
$$ so heißt $\alpha$ eine **lineare Abbildung**. Der Ursprung $O$ des Koordinatensystems $(O,E_1,E_2,E_3)$ ist unter $\alpha$ ein Fixpunkt.[^2]

**Beispiel 3.** Zu bestimmen ist eine analytische Darstellung der Spiegelung (der Punkte) der Ebene an der $x_1$-Achse eines gewählten kartesischen Koordinatensystems.

Bezeichnen $(x_1,x_2)$ die kartesischen Koordinaten eines Punktes $X$ in allgemeiner Lage, so gilt für dessen Spiegelbild $X'=\alpha(X)$
$$
    x_1'=x_1\quad\wedge\quad y_1'=-y_1
$$
Hieraus ergibt sich die Matrixdarstellung der affinen Abbildung $$
  \alpha:\,\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
  \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\cdot
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}
$$
Aus der Darstellung von $\alpha$ ist erkennbar, dass der Translationsvektor $a$ der Nullvektor und die Transformationsmatrix regulär ist, die Abbildung somit eine Affinität und sogar linear ist.

Im nachstehenden Video ist dieses Beispiel im Kontext der linearen Abbildungen erläutert.

!?[Lineare Abbildung](https://www.youtube.com/watch?v=lg1oYbkzauk)

Eine Möglichkeit der Berechnung des Bildes einer Geraden unter einer linearen Abbildung ist an einem Beispiel im nachstehenden Video erläutert.

!?[Lineares Bild](https://www.youtube.com/watch?v=9FBulSYvlF0)


Sicher gewusst?
===============

**Frage 1.** Gegeben sind die folgenden Abbildungsmatrizen affiner Abbildungen
$$
  A_1=\begin{pmatrix} 1 & 2 \\ 2 & 4 \end{pmatrix}\quad\text{sowie}\quad
  A_2=\begin{pmatrix} 1 & 2 & 3 \\ 0 & 1 & 4 \\ -3 & -6 & -9 \end{pmatrix}
$$
Welche dieser beschreibt ~~keine~~ Affinität?

[[X]] $A_1$
[[X]] $A_2$
[[?]] Die Matrix $A$ legt eine Affinität fest, falls die Matrix regulär ist, d. h. $\det{A}\not=0$.
****************************************

Für beide Matrizen gilt $\det{A_1}=\det{A_2}=0$: je zwei Zeilen sind linear abhängig. Daher sind die durch diese Matrizen beschriebenen affinen Abbildungen keine Affinitäten.

****************************************

**Frage 2.** Unter einer affinen Abbildung der Ebene werden abgebildet
$$
  \begin{pmatrix} 0 \\ 0 \end{pmatrix}\mapsto\begin{pmatrix} 1 \\ 2 \end{pmatrix}\,,\quad
  \begin{pmatrix} 1 \\ 0 \end{pmatrix}\mapsto\begin{pmatrix} 3 \\ 2 \end{pmatrix}\,,\quad
  \begin{pmatrix} 0 \\ 1 \end{pmatrix}\mapsto\begin{pmatrix} 2 \\ -4 \end{pmatrix}
$$
Geben Sie die Matrixdarstellung der linearen Abbildung an.

[(X)] $\begin{pmatrix} x' \\ y' \end{pmatrix}=\begin{pmatrix} 2 & 1 \\ 0 & -6 \end{pmatrix}\cdot\begin{pmatrix} x \\ y \end{pmatrix}+\begin{pmatrix} 1 \\ 2 \end{pmatrix}$
[( )] $\begin{pmatrix} x' \\ y' \end{pmatrix}=\begin{pmatrix} 3 & 2 \\ 2 & -4 \end{pmatrix}\cdot\begin{pmatrix} x \\ y \end{pmatrix}+\begin{pmatrix} 1 \\ 2 \end{pmatrix}$
[[?]] Setzen Sie jeweils Urbild- und Bildvektor in die Matrixdarstellung ein.
****************************************

Die Basisvektoren sind $$
  \begin{pmatrix} 1 \\ 0 \end{pmatrix}\quad\text{und}\quad
  \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$ Deren Bilder ergeben sich als Differenzen $$
  \begin{pmatrix} 3-1 \\ 2-2 \end{pmatrix}\quad\text{und}\quad
  \begin{pmatrix} 2-1 \\ -4-2 \end{pmatrix}
$$ und bilden die Spalten der Transformationsmatrix.

****************************************

[^1]: Ist die Transformationsmatrix $A$ eine reguläre Matrix, so ist die dadurch beschriebene affine Abbildung sogar eine Affinität.

[^2]: Ein Punkt $X$ heißt Fixpunkt unter der affinen Abbildung $\alpha$, falls $X=\alpha(X)$, d. h. wenn Bild und Urbild des Punktes übereinstimmen. Für lineare Abbildungen ergibt sich hieraus $$
  \left(x\mapsto x=A\cdot x\right)\quad\leftrightarrow\quad \left(A-E\right)\cdot x=0
$$ worin $E$ die dreireihige Einheitsmatrix bezeichnet. Aus der Darstellung folgt, dass sich die (Ortsvektoren der) Fixpunkte als Eigenvektoren von $A$ zum Eigenwert $1$ berechnen lassen.


### Algebraische Eigenschaften

In diesem Abschnitt werden algebraische Eigenschaften von Affinitäten besprochen. Zentraler Begriff ist die [Transformationsgruppe](https://de.wikipedia.org/wiki/Gruppenoperation) der Affinitäten des $\mathcal{A}^d$, die schrittweise eingeführt wird.

Für die nachfolgenden Betrachtungen bezeichne
$$
  \alpha:\mathcal{A}^d\to\mathcal{A}^d\,,\quad d\in\{2,3\}
$$
eine Affinität, d. h. unter Benutzung von Ortsvektoren bezüglich eines gewählten affinen Koordinatensystems
$$
  \alpha:x\mapsto x'=\alpha(x)=A\cdot x+a
$$
mit regulärer Transformationsmatrix $A\in\mathbb{R}^{d,d}$ ($\det{A}\not=0$) und Translationsvektor $a\in\mathbb{R}^d$.


Komposition
===========

Aus der Schulmathematik ist bekannt, dass die Summe / das Produkt reeller Zahlen wieder eine reelle Zahl ist. Diese sind naheliegende Beispiele für Verknüpfungen.

Im nachstehenden Video werden an Beispielen Verknüpfungen und Verknüpfungsgebilde erläutert.

!?[Verknüpfung](https://www.youtube.com/watch?v=vrh1y9bwhls)

Für beliebige Affinitäten $\alpha_1$ und $\alpha_2$ in $\mathcal{A}^d$ ist eine Verknüpfung, die Hintereinanderausführung (Komposition) zweier Affinitäten, erklärt: Besitzen Affinitäten die Matrixdarstellungen
$$
  \alpha_1:x\mapsto x'=A_1\cdot x+a_1\quad\text{und}\quad
  \alpha_2:x\mapsto x'=A_2\cdot x+a_2
$$
mit $\det{A_i}\not=0$ und $i\in\{1,2\}$, so lässt sich die Komposition[^0] dieser erklären vermöge
$$
  \alpha_2\circ\alpha_1:x\mapsto \alpha_2(\alpha_1(x))
$$
(lies: "$\alpha_2$ nach $\alpha_1$") mit der Matrixdarstellung
$$
  \alpha_2(\textcolor{magenta}{\alpha_1(x)})=A_2\cdot\left(\textcolor{magenta}{A_1\cdot x+a_1}\right)+a_2=B\cdot x+b
$$
worin
$$
  B=A_2\cdot A_1\quad\text{und}\quad b=A_2\cdot a_1+a_2
$$
die Transformationsmatrix beziehungsweise den Translationsvektor bezeichnen. Mit dem [Multiplikationssatz für Determinanten](https://de.wikipedia.org/wiki/Determinante) folgt für die Determinante der Komposition
$$
  \det{B}=\det{(A_2\cdot A_1)}=\det{A_2}\cdot\det{A_1}\not=0
$$
Damit gilt folgender Satz.

>**Satz 1.** Die Komposition zweier Affinitäten in $\mathcal{A}^d$ ist wieder eine Affinität.

**Bemerkung 1.** Eine wichtige Folgerung aus der Komposition von Affinitäten ist, dass die Hintereinanderausführung von beliebigen Affinitäten des $\mathcal{A}^d$ nicht aus der Menge herausführt.

Aus der Eigenschaft, dass die Matrizenmultiplikation im Allgemeinen ~~nicht~~ kommutativ ist, folgt, dass die Komposition von Affinitäten im Allgemeinen ~~nicht~~ kommutativ ist. Es gilt
$$
  \alpha_2\circ\alpha_1=\alpha_1\circ\alpha_2\quad\leftrightarrow\quad
  \left(A_2\cdot A_1=A_1\cdot A_2\quad\wedge\quad A_2\cdot a_1+a_2=A_1\cdot a_2+a_1\right)
$$

Die Eigenschaft der Kommutativität für allgemeine Verknüpfungsgebilde ist an Beispielen im nachstehenden Video erläutert.

!?[Kommutativität](https://www.youtube.com/watch?v=tPAHFqAZ6W4)

**Gegenbeispiel 1.** Die Komposition zweier Translationen der Ebene mit den Matrixdarstellungen
$$
  \tau_1: x\mapsto x'=E\cdot x+t_1 \quad\text{und}\quad
  \tau_2: x\mapsto x'=E\cdot x+t_2
$$
worin $E=\mathrm{diag}{(1,1)}$ die zweireihige Einheitsmatrix sowie $t_i\in\mathbb{R}^2$ mit $i\in\{1,2\}$ die Translationsvektoren bezeichnen, ist offensichtlich kommutativ, d. h. die Reihenfolge der Hintereinanderausführung beider Translationen ist vertauschbar.

Des Weiteren gilt der nachstehende Satz.

>**Satz 2.** Für beliebige Affinitäten $\alpha_1$, $\alpha_2$ und $\alpha_3$ gilt $$
  \alpha_3\circ\left(\alpha_2\circ\alpha_1\right)=\left(\alpha_3\circ\alpha_2\right)\circ\alpha_1
$$ d. h. die Komposition von Affinitäten ist **assoziativ**.[^1]

**Beweis.** Der Beweis vorstehender Aussage nutzt die Assoziativität der Multiplikation von Matrizen aus. Unter Verwendung der Matrixdarstellungen der Affinitäten
$$
  \alpha_i:x\mapsto x'=A_i\cdot x+a_i\,,\quad i\in\{1,2,3\}
$$
ergeben sich einerseits schrittweise die Kompositionen
$$
  \alpha_2\circ\alpha_1:x\mapsto x'=A_2\cdot A_1\cdot x+A_2\cdot a_1+a_2
$$
sowie
$$
  \begin{array}{rrrrl}
    \alpha_3\circ(\alpha_2\circ\alpha_1) & : & x\mapsto x' & = & A_3\cdot(\textcolor{magenta}{A_2\cdot A_1\cdot x+A_2\cdot a_1+a_2})+a_3 \\ & & & = & A_3\cdot A_2\cdot A_1\cdot x+A_3\cdot A_2\cdot a_1 +A_3\cdot a_2+a_3 \\
    & & & = & B\cdot x+b
  \end{array}
$$
worin
$$
  B= A_3\cdot A_2\cdot A_1\quad\text{bzw.}\quad b=A_3\cdot A_2\cdot a_1 +A_3\cdot a_2+a_3
$$
Transformationsmatrix beziehungsweise Translationsvektor beschreiben. Analog bilden sich anderseits die Kompositionen
$$
  \alpha_3\circ\alpha_2:x\mapsto x'=A_3\cdot A_2\cdot x+A_3\cdot a_2+a_3
$$
sowie
$$
\begin{array}{rrrrl}
  (\alpha_3\circ\alpha_2)\circ\alpha_1 & : & x\mapsto x' & = & A_3\cdot A_2\cdot(\textcolor{magenta}{A_1\cdot x+a_1})+A_3\cdot a_2+a_3 \\
  & & & = & A_3\cdot A_2\cdot A_1\cdot x+A_3\cdot A_2\cdot a_1 +A_3\cdot a_2+a_3 \\
  & & & = & B\cdot x+b
\end{array}
$$
worin
$$
C= A_3\cdot A_2\cdot A_1\quad\text{bzw.}\quad c=A_3\cdot A_2\cdot a_1 +A_3\cdot a_2+a_3
$$
Im Vergleich ergeben sich $B=C$ und $b=c$, wonach die Gleichheit aus Satz 2 folgt. $\square$

Die Eigenschaft der Assoziativität für allgemeine Verknüpfungsgebilde ist an Beispielen im nachstehenden Video erläutert.

!?[Assoziativität](https://www.youtube.com/watch?v=uDi_UKcDLdQ)


Existenz der Umkehrabbildung
============================

Da die Transformationsmatrix $A$ zu $\alpha$ regulär angenommen wird, existiert die zu $A$ (multiplikativ) inverse Matrix und es gilt umkehrbar
$$
  x'=A\cdot x+a\quad\leftrightarrow\quad \textcolor{magenta}{A^{-1}}\cdot x'=\textcolor{magenta}{A^{-1}}\cdot A\cdot x+\textcolor{magenta}{A^{-1}}\cdot a \quad\leftrightarrow\quad x=A^{-1}\cdot x'-A^{-1}\cdot a
$$
Daher existiert zu jeder Affinität $\alpha$ eine eindeutig bestimmte **Umkehrabbildung** $\alpha^{-1}$ mit
$$
  \alpha^{-1}:x'\mapsto x=A^{-1}\cdot x'-A^{-1}\cdot a
$$
d. h. jedem Ortsvektor $x$ eines Punktes $X$ kann umkehrbar eindeutig ein Bildvektor $x'$ zu $X'$ zugeordnet werden. Des Weiteren ist die Komposition
$$
  \alpha^{-1}\circ\alpha:\;x\stackrel{\alpha}{\longmapsto}x'\stackrel{\alpha^{-1}}{\longmapsto}x
$$
die identische Abbildung, denn
$$
  x=A^{-1}\cdot\left(A\cdot x+a\right)-A^{-1}\cdot a=A^{-1}\cdot A\cdot x+A^{-1}\cdot a-A^{-1}\cdot a = E\cdot x = x
$$
worin $E=\mathrm{diag}{(1,..,1)}$ die $d$-reihige Einheitsmatrix in $\mathbb{R}^{d,d}$ bezeichnet. Analoges gilt auch für die Komposition
$$
  \alpha\circ\alpha^{-1}:\;x'\stackrel{\alpha^{-1}}{\longmapsto}x\stackrel{\alpha}{\longmapsto}x'
$$

Mit Satz 1 folgen:

>**Satz 3.** Zu jeder Affinität $\alpha$ existiert eine eindeutig bestimmte Umkehrabbildung $\alpha^{-1}$, die wieder eine Affinität ist.

>**Folgerung 4.** Jede Affinität $\alpha$ ist [bijektiv](https://de.wikipedia.org/wiki/Bijektive_Funktion), d. h. injektiv und surjektiv.

**Beispiel 2.** Zur Affinität $\alpha$ mit der Matrixdarstellung
$$
  \alpha:
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
  \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}\cdot
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
  \begin{pmatrix} -2 \\ 3 \end{pmatrix}
$$
ist die Umkehrabbildung $\alpha^{-1}$ zu berechnen.

Die Transformationsmatrix
$$
  A=\begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}
$$
besitzt die inverse Matrix
$$
  A^{-1}=\frac{1}{\det{A}}\cdot\begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}=-\frac{1}{2}\cdot\begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}
$$
denn es gilt einsichtig
$$
  A\cdot A^{-1}=\begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}\cdot\left(-\frac{1}{2}\right)\cdot\begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}=\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}=A^{-1}\cdot A
$$
für die Matrizenmultiplikation.[^2]

Damit lässt sich $\alpha^{-1}$ angeben zu
$$
  \alpha^{-1}:
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}
  \mapsto
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}=\left(-\frac{1}{2}\right)\cdot
  \begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}\cdot
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}-
  \left(-\frac{1}{2}\right)\cdot
  \begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}\cdot
  \begin{pmatrix} -2 \\ 3 \end{pmatrix}
$$
worin sich der Translationsvektor der Umkehrabbildung zu
$$
  b=-\left(-\frac{1}{2}\right)\cdot
  \begin{pmatrix} 4 & -2 \\ -3 & 1 \end{pmatrix}\cdot
  \begin{pmatrix} -2 \\ 3 \end{pmatrix}=
  \begin{pmatrix} 7 \\ -\frac{9}{4} \end{pmatrix}
$$
berechnet.

Die Eigenschaft der inversen Elemente für allgemeine Verknüpfungsgebilde ist an Beispielen im nachstehenden Video erläutert.

!?[Inverses Element](https://www.youtube.com/watch?v=Dmbbash4PrM)


Neutrales Element
=================

In der Menge der Affinitäten des $\mathcal{A}^d$ ist eine Abbildung $\alpha_{id}$ gesucht, für die
$$
  \alpha\circ\alpha_{id}=\alpha_{id}\circ\alpha=\alpha
$$
für alle Affinitäten $\alpha$ des $\mathcal{A}^d$ gilt. Diese wird **neutrales Element** der Komposition genannt. Die Abbildung $\alpha_{id}$ soll sich also wie die "Eins" bei der Multiplikation beziehungsweise wie die "Null" bei der Addition reeller Zahlen verhalten.[^3]

Ihre Existenz folgt unmittelbar aus der nachstehenden Äquivalenz, die sich aus der Existenz der Umkehrabbildung für Affinitäten unter Anwendung des Assoziativgesetzes ergibt.
$$
  \left(\alpha_{id}\circ\alpha\right)\textcolor{magenta}{\circ\alpha^{-1}}=\alpha\textcolor{magenta}{\circ\alpha^{-1}}\quad\leftrightarrow\quad
  \alpha_{id}=\alpha\circ\alpha^{-1}
$$
beziehungsweise
$$
  \textcolor{magenta}{\alpha^{-1}\circ}\left(\alpha\circ\alpha_{id}\right)=\textcolor{magenta}{\alpha^{-1}\circ}\alpha\quad\leftrightarrow\quad
  \alpha_{id}=\alpha^{-1}\circ\alpha
$$
Das neutrale Element ist also die identische Abbildung
$$
  \alpha_{id}:x\mapsto x'=x=E\cdot x+o
$$
deren Transformationsmatrix $E$ die $d$-reihige Einheitsmatrix und deren Translationsvektor $o$ der $d$-dimensionale Nullvektor sind. Offensichtlich gilt $\det{E}=1$, wonach die Matrix $E$ regulär ist.

>**Satz 5.** Es existiert eine eindeutig bestimmte Affinität $\alpha_{id}$ mit $$
  \alpha\circ\alpha_{id}=\alpha_{id}\circ\alpha=\alpha
$$ für alle Affinitäten $\alpha$ des affinen Raumes $\mathcal{A}^d$.

Die Eigenschaft der Existenz des neutralen Elementes für allgemeine Verknüpfungsgebilde ist an Beispielen im nachstehenden Video erläutert.

!?[Neutrales Element](https://www.youtube.com/watch?v=TSXp1yZBZZ4)


Transformationsgruppe der Affinitäten
=====================================

Die Aussagen aus den Sätzen 2, 3 und 5 können benutzt werden, um folgenden Satz zu beweisen.

>**Satz 6.** Die Menge aller Affinitäten $$
  \alpha:\mathcal{A}^d\to\mathcal{A}^d
$$ bildet bezüglich der Komposition eine **Gruppe**.

**Bemerkung 2.** Der Begriff einer Gruppe ist eine mathematische Struktur. Andere mathematische Strukturen sind beispielsweise *Körper* oder *Vektorraum*. Die axiomatische Definition einer Gruppe ist in nachstehendem Video erläutert.

!?[Gruppe](https://www.youtube.com/watch?v=LyFCkZXkly0)

Im Rahmen dieser Vorlesungen werden Transformationsgruppen benutzt, um beispielsweise periodische Muster, so genannte Ornamente zu analysieren beziehungsweise aus einem Grundmuster zu erzeugen.

**Beispiel 3.** Ein Beispiel für ein Ornament ist in nachfolgender Abbildung dargestellt. Es ist ein ~~Fries~~ (Bandornament). Siehe https://de.wikipedia.org/wiki/Friesgruppe.

![Fries](img/Fries.png "Periodische Fortsetzung der Grundeinheit durch wiederholte Translation $\tau$ mit fester Translationsrichtung und festem Translationsbetrag.")

Die periodische Fortsetzung der Grundeinheit wird durch wiederholte Translation $\tau$ mit fester Translationsrichtung und festem Translationsbetrag erzeugt.

Die Menge aller Abbildungen, die sich wiederholtes Verknüpfen von $\tau$ (und ihrer Umkehrabbildung) ergeben, bildet die Gruppe dieses Frieses, dessen **Friesgruppe**. Die von $\tau$ erzeugte Friesgruppe enthält sowohl die $k$-fachen Kompositionen
$$
  \tau^k:=(...((\tau\circ\tau)\circ\tau)\circ...)\circ\tau
$$
worin $k\in\mathbb{N}$, als auch
$$
  \tau^{-k}:=(...((\tau^{-1}\circ\tau^{-1})\circ\tau^{-1})\circ...)\circ\tau^{-1}
$$
Mit $\tau^{-1}\circ\tau$ ist auch die identische Abbildung in der Friesgruppe enthalten.

Wird die Grundeinheit "ausgetauscht", so entsteht ein anderes Fries zur selben Friesgruppe.

Eine interaktive Möglichkeit, Friese aus einer Grundeinheit durch Anwenden verschiedener Friesgruppen zu erzeugen, finden Sie unter [Friesgruppen](https://www.geogebra.org/m/wjpjzrce). Viel Spaß damit ;)

>**Definition.** Sei $\alpha$ mit $$
  \alpha:x\mapsto x'=A\cdot x+a
$$ eine Affinität. Ist $A=E=\mathrm{diag}{(1,...,1)}$ die $d$-reihige Einheitsmatrix, so heißt $$
  \alpha:x\mapsto x'= x+a
$$ **Translation** mit Translationsvektor $a$.

>**Satz 7.** Jede Affinität des $d$-dimensionalen affinen Raumes $\mathcal{A}^d$ ist Komposition einer linearen Abbildung und einer Translation.[^4]

**Beweis.** Eine Affinität mit der Matrixdarstellung
$$
  \alpha:x\mapsto x'=A\cdot x+a\,,\quad\det{A}\not=0
$$
worin $A\in\mathbb{R}^{d,d}$ die (reguläre) Transformationsmatrix und $a\in\mathbb{R}^d$ den Translationsvektor bezeichnen, lässt sich als Komposition
$$
  \tau\circ\gamma:x\mapsto x''=E\cdot(\textcolor{magenta}{A\cdot x})+a
$$
der Affinitäten
$$
  \gamma:x\mapsto x'=A\cdot x\quad\text{sowie}\quad \tau:x'\mapsto x''=E\cdot x'+a
$$
darstellen, worin $E$ die $d$-reihige Einheitsmatrix beschreibt. $\square$


Sicher gewusst?
===============

**Frage 1.** Entscheiden Sie, ob die folgende affine Abbildung umkehrbar ist.
$$
  \alpha:\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
  \begin{pmatrix} 1 & -2 \\ -2 & 4 \end{pmatrix}\cdot
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
  \begin{pmatrix} 7 \\ -3 \end{pmatrix}
$$

[( )] Die Umkehrabbildung $\alpha^{-1}$ zu $\alpha$ kann dargestellt werden durch $$ \alpha^{-1}:\begin{pmatrix} x_1' \\ x_2' \end{pmatrix}\mapsto
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}=
  \begin{pmatrix} 1 & -2 \\ -2 & 4 \end{pmatrix}^{-1}\cdot
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}-
  \begin{pmatrix} 1 & -2 \\ -2 & 4 \end{pmatrix}^{-1}\cdot
  \begin{pmatrix} 7 \\ -3 \end{pmatrix}
$$ und existiert also.
[(X)] Die Abbildung $\alpha$ ist nicht umkehrbar.
[[?]] Die Umkehrbarkeit von affinen Abbildungen $\alpha$ des $d$-dimensionalen affinen Raumes $\mathcal{A}^d$ ist an die Regularität der Transformationsmatrix gekoppelt.
****************************************

Die Determinante der Transformationsmatrix berechnet sich $$ \det{\begin{pmatrix} 1 & -2 \\ -2 & 4 \end{pmatrix}}=1\cdot 4-(-2)^2=0 $$ wonach die Transformationsmatrix nicht regulär ist. Die inverse Matrix $$ \begin{pmatrix} 1 & -2 \\ -2 & 4 \end{pmatrix}^{-1} $$ existiert nicht, die Umkehrabbildung demnach auch nicht.

****************************************

**Frage 2.** Ist die Komposition der beiden affinen Abbildungen $$
\alpha_1:\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
\begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}
$$ sowie $$
\alpha_2:\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
\begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
\begin{pmatrix} 1 \\ 1 \end{pmatrix}
$$ kommutativ?

[(X)] Ja.
[( )] Nein.
[[?]] Es ist zu prüfen, ob hier $$ \alpha_1\circ\alpha_2=\alpha_2\circ\alpha_1 $$ gilt. Berechnen Sie zur Entscheidung der Frage die Matrixdrstellungen von $\alpha_1\circ\alpha_2$ und $\alpha_2\circ\alpha_1$.
****************************************

Es gilt einerseits $$ \alpha_1\circ\textcolor{magenta}{\alpha_2}: \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
\begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\cdot\left(\textcolor{magenta}{
\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
\begin{pmatrix} 1 \\ 1 \end{pmatrix}}\right)=
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
\begin{pmatrix} 1 \\ 1 \end{pmatrix}
$$ Der Translationsvektor von $\alpha_2$ ist offenbar ein Eigenvektor der Transformationsmatrix von $\alpha_1$ zum Eigenwert Eins. Andererseits berechnet sich $$ \alpha_2\circ\textcolor{magenta}{\alpha_1}: \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
\begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
\begin{pmatrix} 1 & 0 \\ 0 & 1  \end{pmatrix}\cdot\left(\textcolor{magenta}{
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}}\right)+
\begin{pmatrix} 1 \\ 1 \end{pmatrix}=
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}\cdot
\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
\begin{pmatrix} 1 \\ 1 \end{pmatrix}
$$ d. h. mit gleichem Ergebnis.[^5]

****************************************

[^0]: Das Verknüpfungszeichen der Komposition ist "$\circ$" und sollte nicht mit dem Verknüpfungssymbol beim Bilden des Skalarproduktes verwechselt werden.

[^1]: Diese Aussage gilt allgemeiner sogar für affine Abbildungen in $\mathcal{A}^d$.

[^2]: Das Produkt zweier quadratischer Matrizen gleichen Typs im Allgemeinen ~~nicht~~ kommutativ ist.

[^3]: Die Existenz einer solchen Affinität $\alpha_{id}$ ist für den Nachweis der Gruppeneigenschaft der Affinitäten des $\mathcal{A}^d$ von struktureller Bedeutung.

[^4]: Diese Aussage gilt sogar allgemeiner für affine Abbildungen.

[^5]: Im Allgemeinen ist die Hintereinanderausführung zweier Abbildungen nicht kommutativ.


### Geometrische Eigenschaften

In diesem Abschnitt werden ausgewählte geometrische Eigenschaften von Affinitäten des $d$-dimensionalen Raumes $\mathcal{A}^d$ untersucht.

Eine Eigenschaft ist an den Erhalt unter Affinitäten gekoppelt, in diesem Sinne eine affine Eigenschaft. Beispielsweise werden Parallelogramme unter Affinitäten auf Parallelogramme abgebildet: Die Parallelität von Geraden bleibt unter Affinitäten erhalten, Seitenlängen oder Winkelgrößen in einer geometrischen Figur hingegen im Allgemeinen nicht.

In diesem Sinne ist auch die Kongruenz (Deckungsgleichheit) geometrischer Figuren an Längen und Winkel erhaltende Abbildungen gekoppelt, die sogenannten Kongruenzen. Diese werden an späterer Stelle untersucht.


Erhalt von Geraden
==================

Eine Gerade $g$ des $d$-dimensionalen Raumes $\mathcal{A}^d$, $d=\{2,3\}$, lässt sich mithilfe einer Parameterdarstellung beschreiben
$$
  g:\;x=s+\lambda\cdot t\,,\quad \lambda\in\mathbb{R}
$$
worin $s\in\mathbb{R}^d$ den Ortsvektor eines Punktes der Geraden (Stützvektor) und $t\in\mathbb{R}^d\setminus\{o\}$ einen Richtungsvektor der Geraden bezeichnet.

Das Bild dieser Gerade unter einer Affinität
$$
  \alpha:\mathcal{A}^d\to\mathcal{A}^d,x\mapsto x'=A\cdot x+a
$$
mit regulärer Transformationsmatrix $A\in\mathbb{R}^{d,d}$ und Translationsvektor $a\in\mathbb{R}^d$ berechnet sich zu
$$
  g':\;x'=A\cdot\left(s+\lambda\cdot t\right)+a=s'+\lambda\cdot A\cdot t
$$
worin $s'=\alpha(s)$ das Bild von $s$ unter $\alpha$ und $A\cdot t$ als affines Bild eines "Differenzvektors" gedeutet werden kann (Siehe Eigenschaft 5 im Abschnitt [Definition und allgemeine Eigenschaften](#Definition-und-allgemeine-Eigenschaften).) Das heißt $x'$ ist wieder Parameterdarstellung einer Geraden, sofern $A\cdot t\not=o$ (Nullvektor). Es gilt
$$
  \forall\, t\not=o\;\left(A\cdot t\not=o\right)\quad\leftrightarrow\quad \det{A}\not=0
$$
Die Gerade $g'$ heißt die Bildgerade von $g$ unter der Affinität $\alpha$.

**Beispiel 1.** Gegeben ist die Parameterdarstellung einer Geraden $g$ in der Ebene $\mathbb{R}^2$
$$
  g:\;\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}=
  \begin{pmatrix} 2 \\ 3 \end{pmatrix}+
  \lambda\cdot\begin{pmatrix} 2 \\ 1 \end{pmatrix}\,,\quad\lambda\in\mathbb{R}
$$
Zu berechnen ist das Bild $g'$ der Geraden $g$ unter der Affinität
$$
  \alpha:\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}\mapsto
  \begin{pmatrix} x_1' \\ x_2' \end{pmatrix}=
  \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\cdot
  \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}+
  \begin{pmatrix} 2 \\ 0 \end{pmatrix}
$$
Dies ist eine Spiegelung der Ebene an der $x_1$-Achse ($x_2=0$) mit anschließender Translation in Richtung dieser Achse.[^1] Sie nachstehende Abbildung

![Spiegelung](img/geo-bild07.png "Gerade $g$ und deren Bildgerade $g'$ unter der Affinität $\alpha$: Diese ist die Hintereinanderausführung der Spiegelung an der $x_1$-Achse und anschließender Translation in Richtung $a=(2,0)^\top$.")

Das affine Bild $s'$ des Stützvektors $s$ von $g$ unter $\alpha$ besitzt die Koordinatendarstellung
$$
  \begin{pmatrix} s_1' \\ s_2' \end{pmatrix}=
  \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\cdot
  \begin{pmatrix} 2 \\ 3 \end{pmatrix}+
  \begin{pmatrix} 2 \\ 0 \end{pmatrix}=
  \begin{pmatrix} 4 \\ -3 \end{pmatrix}
$$
das affine Bild $t'$ des Stützvektors $t$ von $g$ (als Differenzvektor)
$$
  \begin{pmatrix} t_1' \\ t_2' \end{pmatrix}=
  \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}\cdot
  \begin{pmatrix} 2 \\ 1 \end{pmatrix}=
  \begin{pmatrix} 2 \\ -1 \end{pmatrix}
$$
Die Bildgerade $g'$ besitzt damit eine Parameterdarstellung
$$
  g':\;\begin{pmatrix} x_1 \\ x_2 \end{pmatrix}=
  \begin{pmatrix} 4 \\ -3 \end{pmatrix}+
  \lambda\cdot\begin{pmatrix} 2 \\ -1 \end{pmatrix}\,,\quad\lambda\in\mathbb{R}
$$

Die Berechnung der Bildgerade unter einer Affinität ist an einem Beispiel im nachstehenden Video erläutert.

!?[Bildgerade](https://www.youtube.com/watch?v=9FBulSYvlF0)

**Bemerkung 1.** Für die Berechnung des Richtungsvektors $t'$ von $g'$ ist ~~lediglich~~ die Transformationsmatrix $A$ von links mit $t$ zu multiplizieren. Die Äquivalenz  
$$
  \forall\, t\not=o\;\left(A\cdot t\not=o\right)\quad\leftrightarrow\quad \det{A}\not=0
$$
lässt sich mit Hilfe des Kerns einer linearen Abbildung deuten, welche der quadratischen Matrix $A$ zugeordnet ist.

Im nachstehenden Video ist der Kern[^2] einer linearen Abbildung / Matrix an einem Beispiel erläutert.

!?[Kern einer linearen Abbildung](https://www.youtube.com/watch?v=1JpqMmOSDoI)

**Bemerkung 2.** Wie der Erhalt von Geraden unter Affinitäten lässt sich der **Erhalt von Ebenen** unter einer Affinität im dreidimensionalen Raum beweisen.

Aus beiden Eigenschaften folgt, dass unter Affinitiäten der Ebene / des dreidimensionalen Raumes

1. Dreiecke auf Dreiecke / ebene Polygone auf ebene Polygone mit gleicher Seiten- und Eckenzahlen
2. [Polyeder](https://de.wikipedia.org/wiki/Polyeder) auf Polyeder mit gleichen Flächen-, Kanten- und Eckenzahlen etc.

abgebildet werden. Siehe auch das interaktive Beispiel [Bild unter Affinität](https://www.geogebra.org/m/mZ8N3n6N).


Erhalt von Parallelität
=======================

Parallele Geraden werden unter einer Affinität auf parallele Geraden abgebildet, d. h.
$$
  g\parallel h\quad\stackrel{\alpha}{\longrightarrow}\quad g'\parallel h'
$$
für $g'=\alpha(g)$ und $h'=\alpha(h)$. Es gilt nämlich für Richtungsvektoren der Geraden $g$ und $h$
$$
  t_g=\lambda\cdot t_h
$$
für einen Parameterwert $\lambda\in\mathbb{R}\setminus\{0\}$: Die Vektoren $t_g$, $t_h$ sind linear abhängig. Für deren affine Bilder folgt
$$
  t_g'=A\cdot t_g=A\cdot\left(\lambda\cdot t_h\right)=\lambda\cdot\left(A\cdot t_h\right)=\lambda\cdot t_h'
$$
d. h. aus der linearen Abhängigkeit der Urbilder folgt die lineare Abhängigkeit der Bilder unter der Affinität
$$
  \alpha:\mathcal{A}^d\to\mathcal{A}^d,x\mapsto x'=A\cdot x+a
$$
mit $\det{A}\not=0$.


Erhalt des Teilverhältnisses
============================

Da das Bild einer Geraden unter einer Affinität wieder eine Gerade ist, stellt sich die Frage, wie sich das Teilverhältnis von drei Punkten der Gerade transformiert.

Für die folgende Betrachtung bezeichnen $P$, $Q$ und $T$ drei Punkte auf einer Geraden $g$ mit dem Teilverhältnis
$$
  TV(P,Q,T)=\lambda\quad\leftrightarrow\quad t=\frac{1}{1-\lambda}\cdot p-\frac{\lambda}{1-\lambda}\cdot q
$$
vergleiche Abschnitt [Teilungspunkt einer Strecke](#Teilungspunkt-einer-Strecke).

Werden die Punkte $P$, $Q$ und $T$ unter einer Affinität
$$
  \alpha:\mathcal{A}^d\to\mathcal{A}^d,x\mapsto x'=A\cdot x+a
$$
mit $\det{A}\not=0$ abgebildet auf die Punkte $P'$, $Q'$ und $T'$ mit Ortsvektoren
$$
  p'=A\cdot p+a\,,\quad q'=A\cdot q+a\quad\text{und}\quad
  t'=A\cdot t+a
$$
so folgt für die Konvexkombination
$$
  \frac{1}{1-\lambda}\cdot p'-\frac{\lambda}{1-\lambda}\cdot q'=
  \frac{1}{1-\lambda}\cdot (A\cdot p+a)-\frac{\lambda}{1-\lambda}\cdot (A\cdot q+a)=a+A\cdot\left(\frac{1}{1-\lambda}\cdot p-\frac{\lambda}{1-\lambda}\cdot q\right)=t'
$$
d. h. es gilt allgemein für drei Punkte auf einer Geraden
$$
TV(P,Q,T)=TV(P',Q',T')  
$$
falls dieses erklärt ist.

**Bemerkung 3.** Da das Teilverhältnis von drei Punkten auf einer Geraden unter Affinitäten erhalten bleibt, wird insbesondere der Mittelpunkt einer Strecke auf den Mittelpunkt der Bildstrecke abgebildet.

**Beispiel 2.** Ein Kreis $k$ zur Gleichung $x_1^2+x_2^2=1$ besitzt den Mittelpunkt $O(0,0)$ und den Radius $r=1$. Dieser wird unter der Affinität
$$
  \alpha:x\mapsto x'=B\cdot x+b
$$
mit
$$
  B=\begin{pmatrix} 2 & 0 \\ 0 & 3 \end{pmatrix}\quad\text{und}\quad
  b=\begin{pmatrix} 1 \\ -1 \end{pmatrix}
$$
vermöge
$$
  x_1'=2\cdot x+1\;\wedge\; x_2'=3\cdot x_2-1\quad\rightarrow\quad
  x_1=\frac{1}{2}\cdot(x_1'-1)\;\wedge\; x_2=\frac{1}{3}\cdot(x_2'+1)
$$
abgebildet auf eine Ellipse $k'$ mit der quadratischen Gleichung
$$
  \frac{(x_1'-1)^2}{4}+\frac{(x_2'+1)^2}{9}=1
$$
Dies ist die Gleichung einer Ellipse mit Mittelpunkt $M'(1,-1)$ in achsenparalleler Lage mit den Halbachsenlängen $a_1=2$ und $a_2=3$.


Ein interaktives Beispiel der Affinität eines Kreises auf eine Ellipse ist unter [Ellipse als affines Bild eines Kreises](https://www.geogebra.org/m/RkhYxK5m) erläutert.


Erhalt des Schwerpunktes
========================

>**Satz.** Gegeben ist ein System von Massepunkten $P_i$, $i\in\{1,...,n\}$, mit den Ortsvektoren $p_i$ und Gewichten $\gamma_i\geq0$. Der Schwerpunkt $S$ dieses Massesystems besitzt den Ortsvektor $$
  s=\frac{1}{\sum_{j=1}^n{\gamma_j}}\cdot\sum_{i=1}^n{\gamma_i\cdot p_i}
$$ Das Bild $\alpha(S)$ unter einer Affinität $\alpha:x\mapsto x'=A\cdot x+a$ ist Schwerpunkt des Massesystems $$
  \alpha(P_1)\,,...\,,\alpha(P_n)
$$ mit den Gewichten $\gamma_i$.

**Beweis.** Der Schwerpunkt $S'$ des Massesystems
$$
  \alpha(P_1)\,,...\,,\alpha(P_n)
$$
mit den Gewichten $\gamma_i$ besitzt den Ortsvektor
$$
  s'=\frac{1}{\sum_{j=1}^n{\gamma_j}}\cdot\sum_{i=1}^n{\gamma_i\cdot(A\cdot p_i+a)}=
  \frac{1}{\sum_{j=1}^n{\gamma_j}}\cdot\left[\left(\sum_{i=1}^n{\gamma_i}\right)\cdot a+A\cdot\sum_{i=1}^n{\left(\gamma_i\cdot p_i\right)}\right]=\alpha(s)
$$

$\square$


Affines Bild einer Bézierkurve
==============================

>**Satz.** Das Bild einer Bézierkurve $c$ mit den Kontrollpunkten $$
  P_0\,,\, P_1\,,\,...\,,\,P_n
$$ unter einer Affinität $\alpha:x\mapsto x'=A\cdot x+a$ ist die Bézierkurve zum affinen Bild $$
  \alpha(P_0)\,,\alpha(P_1)\,,...\,,\alpha(P_n)
$$ der Kontrollstruktur.

**Beweis.** Das affine Bild der Bézierkurve berechnet sich unter Verwendung der Ortsvektoren $p_i$ der Kontrollpunkte $P_i$
$$
  \alpha(c)=A\cdot\left(\sum_{i=0}^n{\left(B_i^n(t)\cdot p_i\right)}\right)+a
$$
Zu zeigen ist, dass sich diese Darstellung in die folgende äquivalent überführen lässt
$$
  \alpha(c)=\sum_{i=0}^n{\left(B_i^n(t)\cdot(A\cdot p_i+a)\right)}=\sum_{i=0}^n{\left(B_i^n(t)\cdot\alpha(p_i)\right)}
$$
d. h. die Wirkung von $\alpha$ auf $c$ lässt sich durch die affine Transformation der Kontrollpunkte $\alpha(P_i)$ beschreiben.

Unter Anwendung des Satzes 2 aus dem Abschnitt [De Casteljau Algorithmus](#De-Casteljau-Algorithmus) kann der Translationsvektor $a$
$$
  a=a\cdot 1=a\cdot\sum_{i=0}^n{B_i^n(t)}\quad\forall\; t\in[0,1]
$$
Unter Verwendung der Rechenregeln für Summen folgt hieraus
$$
  \alpha(c)=\sum_{i=0}^n{\left(B_i^n(t)\cdot A\cdot p_i\right)}+\sum_{i=0}^n{\left(B_i^n(t)\cdot a\right)}=\sum_{i=0}^n{\left(B_i^n(t)\cdot(A\cdot p_i+a)\right)}
$$
und damit die Behauptung. $\square$

Die Möglichkeit einer interaktiven Erzeugung von drehsymmetrischen Ornamenten unter Benutzung von Bézierkurven ist unter [Bézier-Rosette](https://www.geogebra.org/m/YdNJrPfp) zu probieren.

[^1]: Diese Affinität hat die zusätzliche Eigenschaft, Längen von Strecken / Kurven zu erhalten, des Weiteren die Größe von Winkeln. Sie gehört zu den so genannten Kongruenzen.

[^2]: Der Kern $\ker{\alpha}$ einer linearen Abbildung $\alpha$ ist die Menge der Vektoren $x$, die durch $\alpha$ auf den Nullvektor $o$ abgebildet werden, d. h. alle Lösungen der Gleichung $\alpha(x)=o$.
