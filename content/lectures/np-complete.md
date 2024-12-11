+++
date = '2024-12-11'
draft = true
title = 'NP Vollständigkeit'
+++

{{< mathjax >}}

NP-Hard und NP-Complete (NP-Schwer und NP-Vollständig) sind eine der wichtigsten Komplexitätsklassen in der Informatik. Ermöglicht durch Cook's arbeiten zu der Praxis des _Reduzierens_ und des finden eines initialen NP-Vollständigen Problems (1971)[^1].

Schauen wir uns zunächst eines der bekannten NP-Vollständigen Probleme an. Das
**Rundreiseproblem**
> **Definition Rundreiseproblem:**
> - **gegeben** Eine Menge aus Entfernungen mit $n \times n$ Elementen und eine Zahl $k$. Jedes Element $M_{i,j}$ beschreibt dabei die Entfernung von einer Stadt Nummer $i$ zu Stadt Nummer $j$.
> - **gefragt** Gibt es eine "_Rundreise_" deren Wegstrecke kleiner als $k$ ist. Wichtig hierbei, dass jede Stadt genau einmal besucht wird.
> Also gibt es eine Permutation $\pi$, so dass gilt: $(\sum_{i=1}^{n-1} M_{\pi(i), \pi{i+1}}) + M_{\pi(n), \pi(1)} \leq k$.

Eine Funktion, die dieses Rundreiseproblem berechnet kann beispielsweise mit folgender Funktion gelöst werden:

```Java
int k = ...;
int[][] M = ...; // Matrix aus n x n Entfernungen
for(i1 in {1,2,3,4,5 ... n}){
    for(i2 in {1,2,3,4,5 ... n}){
        for(i3 in {1,2,3,4,5 ... n}){
          ...
          for(iN in {1,2,3,4,5 ... n}){
            int distance = M[i1][i2] + M[i2][i3] + ... + M[iN][i1];
            if(distance < k){
                return true;
            }
        }   
    }  
}
return false;
```

Diese Funktion hat allerdings ein Laufzeitverhalten von $O(n^n)$.
Die Rechenzeit steigt exponentiell mit der Anzahl der Städte.

Die Frage ist nun, ob es auch einen Algorithmus gibt, welcher das Rundreiseproblem in Polynomialer Laufzeit löst.

Um diese Frage zu klären gibt es die durch Cook 1971 eingeführte Aufteilung in P und NP schwere Probleme und eine "_polynomiales Reduzieren_" genannte Beweismethode.
So lässt sich Beispielsweise SAT auf das Rundreiseproblem reduzieren.
Es gilt:

> SAT $\leq_p$ Rundreiseproblem

Dadurch wissen wir, dass das Rundreiseproblem mindestens so "_schwer_" ist wie das Erfüllbarkeitsproblem der Aussagenlogik und dadurch gibt es momentan keinen bekannten Algorithmus der dieses Problem in polynomialer Laufzeit $O(n^x)$ löst.

### P und NP

* **P** bedetet eine deterministische Turingmaschine kann es in $O(n^x)$ lösen.
Oder anders formuliert:
Es gibt ein Polynom[^3] $p$ und eine Deterministische Turingmaschine $M$ mit der gilt: $p(n) \leq $ {Anzahl der Rechenschritte von $M$ auf einer Eingabe der Länge $n$}.

* **NP** schwere Probleme sind Probleme für die es eine nichtdeterministische Turingmaschine gibt, welche es in $p(n)$ Schritten lösen kann:

>**Definition:** 
>
> _ntime_$_M(x) = \begin{cases}\text{min}[\text{Anzahl Schritte von}\ M\ \text{auf}\ x], & x \in T(M) \\ 0, & x \notin T(M)\end{cases}$
>
> _Bemerkung:_ hierbei ist die Anzahl der Schritte einer NDTM bei einer akzeptierenden Rechnung gemeint.


## NP-Vollständigkeit
Wie zeigen wir, ob ein Problem NP-schwer ist? Durch **Polynomiale Reduzierung** auf ein NP-Vollständiges Problem:

> **Definition:**
> Seien A $\subseteq \Sigma^* $ und B $\subseteq \Gamma^* $ Sprachen.
> Dann heißt _"A auf B polynomial reduzierbar"_ - ausgedrückt A $\leq_p$ B -
> falls es eine totale und mit polynomialer Komplexität berechnbare Funktion $f : \Sigma^* \to \Gamma^* $ gibt, so dass für alle $x \in \Sigma^*$ gilt:
> 
> - $x \in$ A $\implies$ $f(x) \in$ B
> - $f(x) \in$ B $\implies$ $x \in$ A 

* $\leq_p$ ist transitiv
  - A $\leq_p$ B und B $\leq_p$ C $\implies$ A $\leq_p$ C
* Daher: Um zu zeigen, dass ein Problem B NP-Schwer ist, reicht es eine polynomiale Reduktion auf ein anderes NP-schweres Problem A zu finden (B $\leq_p$ A)
  * Da $\leq_p$ transitiv ist, können somit alle NP-Schweren Probleme auf B reduziert werden
* der Trick ist es, ein NP-Vollständiges Problem als initiales Problem zu finden.
    - anschließend können alle anderen Probleme auf dieses reduziert werden.
    - Dies ist das SAT Problem und wurde von Cook bewiesen (1971)


> Falls A $\leq_p$ B und B $\in$ NP, dann ist auch A $\in$ NP.

- Es stellt sich heraus, dass alle NP-schweren Probleme aufeinander reduzierbar sind.
- findet man einen effektiven Algorithmus für ein NP-schweres Problem, so findet man einen für alle NP-schweren Probleme. Dann wär P = NP
    - der Grund ist die transitivität der polynomiellen Reduzierung


> Sei A NP-Vollständig. Dann gilt: A $\in$ P $\Leftrightarrow$ P = NP

_Bemerkung:_

* NP-Schwer bedeutet, ein Problem ist mindestens so schwer wie alle Probleme, welche von einer Nichtdeterministischen Turingmaschine in polynomialer Laufzeit berechenbar sind.
  * es könnte allerdings auch schwerer sein
  * Um NP-Vollständigkeit zu zeigen reicht es meist aus zu zeigen, dass das Problem NP-Schwer ist und in NP enthalten ist.
  * _Erinnerung:_ Die Klasse NP enthält alle Probleme, welche mit einer Nichtdeterministischen Turinmaschine in polynomialer Laufzeit berechenbar sind.


### SAT Problem und Satz von Cook
Das Erfüllbarkeitsproblem der Aussagenlogik (kurz SAT) ist das folgende:
Gegeben sei eine Aussagenlogische Formel[^2] F. Gibt es eine Belegung, welche diese Formel erfüllt.

> Das Erfüllbarkeitsproblem der Aussagenlogik ist NP-Vollständig

**Beweis:**

_Bemerkung:_ Der Trick hier ist, dass alle Probleme in NP immer in $p(|x|)$ Schritten berechenbar sind. Für jede Eingabe $x$ lässt sich also eine SAT Formel generieren,  die alle $p(|x|)$ Schritte einer Turingmaschine abbildet, welche zu einer akzeptierenden Belegung für $x$ führen.

### Zusammenfassung
- Für Probleme in P gibt es in der Praxis Algorithmen, welche ein akzeptables Laufzeitverhalten haben.
- Probleme in NP sind in der Praxis meist nicht berechenbar. Sie haben ein Laufzeitverhalten in der Größenordnung von O(2^n) (also exponentiell wachsend mit der Eingabegröße).
- Dies konnte aber nie bewiesen werden. Es ist immer noch unbekannt ob **P $\neq$ NP** oder **P = NP**:
  - Es könnte immer noch eine Möglichkeit geben ein Problem aus NP in polynomialer Laufzeit zu lösen, was bedeuten würde, dass alle Probleme in NP nun in polynomialer Laufzeit lösbar sind, wodurch P = NP.
  - bisher gilt aber in der Praxis immer noch P $\neq$ NP

### Typinferenz für FGJ ist NP-Schwer

Gegeben sein eine Sprache TI-Java, welche wie folgt definiert ist:
- **Gegeben:** ein Java Program mit fehlenden Typannotationen.
- **Gefragt:** Gibt es eine Typeinsetzung für die fehlenden Annotationen, so dass ein korrektes Java Programm entsteht?

#### Reduktion auf SAT #### 

Wir zeigen, dass sich SAT auf _Typinferenz für Java_ (TI-Java) reduzieren lässt.

Dazu geben wir eine Funktion $f$ an, welche jede erfüllbare Formel F in ein TI-Java Programm umwandelt.
Wir geben diese Funktion hier beispielhaft an. Sei beispielsweise die Eingabe F $= (x_1 \wedge x_2) \vee (x_2 \wedge \neg x_3)$

Daraus erzeugt $f$ das Java-TI Programm:
```Java
class True extends Object{
  False not(){ return new False(); }
  True and(True t){ return t; }
  False and(False f){ return f; }
  True or(Object any){ return this; }
}
class False extends Object{
  True not(){ return new True(); }
  False and(Object any){ return this; }
  True of(True t){ return t;}
}

class SATEncoding extends Object{
  True sat(x1, x2, x3, x1, x2){
    return (x1.and(x2)).or(x2.and(x3.not()));
  }
}
```
Ist dieses Programm ein korrektes Java-TI Programm, so gibt es eine Einsetzung von Typen in die fehlenden Methodenparameter der `sat` Methode.
Diese müssen dabei so gewählt sein, dass die durch Java-Code ausgedrückte Aussagenlogische Formel im `return` Statement den Typ `True` zurückliefert.

Es gilt nun:

1. Ein SAT Problem F lässt sich in polynomialer Zeit in eine Java Methode umwandeln.
Die Funktion $f$ hat offensichtlich eine Laufzeit von $O(n)$, wobei $n$ die Anzahl der Klauseln in der Formel F sind.

2. Falls $f$(F) $\in$ TI-Java$\implies$ F $\in$ SAT: Wird das Java Programm nach der Typinferenz - also dem Einsetzen der fehlenden Typannotationen - akzeptiert, so muss es auch eine Variablenbelegung für F geben, welche F erfüllt.

3. Falls F $\in$ SAT $\implies$ $f$(F) $\in$ TI-Java: Gibt es eine Variablenbelegung für F, so muss es auch eine Typisierung für das erzeuge Java Programm geben.

### NP-Schwere Probleme die nicht in NP sind
Es gibt Probleme, die zwar NP-Schwer sind, allerdings nicht in NP liegen,
also "_schwerer_" als NP-Vollständige Probleme sein müssen.

* So sind Unentscheidbare Probleme generell nicht in NP:
  - Eine deterministische Turinmaschine kann eine NDTM simulieren.
  - könnte eine NDTM eine unberechenbares Problem in polynomialer Laufzeit lösen, so könnte dies auche eine DTM in endlicher Zeit.


- **Beispiel:** Das Wortproblem für Typ-0 Sprachen:
  * nicht in NP, da es unentscheidbar ist
  * allerdings NP-Schwer, da SAT durch eine Typ-0 Grammatik bzw. einer Turingmaschine ausgerechnet werden kann und somit eine Untermenge des Wortproblems für Typ-0 Sprachen ist.

[^1]: Stephen A. Cook. 1971. The complexity of theorem-proving procedures. In Proceedings of the third annual ACM symposium on Theory of computing (STOC '71). Association for Computing Machinery, New York, NY, USA, 151–158. https://doi.org/10.1145/800157.805047

[^2]: beispielsweise $(x_1 \wedge x_2) \vee (x_2 \wedge \neg x_3)$, wobei die Variablen $x$ jeweils die Wahrheitswerte _Wahr_ oder _Falsch_ annehmen können.

[^3]: Ein Polynom ist eine Funktion der Form $a x^n + bx^{n-1} + .. + cx + d$