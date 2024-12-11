+++
date = '2024-12-11T01:06:45+01:00'
draft = true
title = 'Np Completeness'
+++

# Traveling Salesman

```Java
int travelingSalesman(int currentNode, int distanceTraveled, int[] visitedNodes){
    if(visitedNodes.length == numNodes) return distanceTraveled;
    for(i = 0; i < numNodes; i++){
        int nextNode = i;
        int totalDistance = distanceTraveled + distance(currentNode, i);
        return 
    }
}
```

Ein Graph mit 5 Knoten kann auch mit fünf ineinander verschachtelten For-Schleifen berechnet werden.

```Java
int shortestDistance = -1;
int[] shortestPath = []
for(i in {1,2,3,4,5}){
    for(i2 in {1,2,3,4,5}){
        for(i3 in {1,2,3,4,5}){
            ...
            int totalDistance = dikstra(i1, i2, i3, i4, i5)
            if(totalDistance < shortestDistance){
                shortestDistance = totalDistance
                shortestPath = [i1,i2,i3,i4,i5]
            }
        }   
    }  
}
```

Die Frage ist nun, gibt es auch einen Algorithmus, welcher das Traveling Salesman Problem in Polynomieller Laufzeit löst.

Um diese Frage zu klären gibt es die Cook 1971 eingeführte Aufteilung in P und NP schwere Probleme.
P bedetet eine deterministische Turingmaschine kann es in O(n^x) lösen.
Es gibt ein Polynom p mit p(n)<= Anzahl Schritte einer Turingmaschine mit eingabe der Länge n.

NP schwere Probleme sind Probleme für die es eine nichtdeterministische Turingmaschine gibt, welche es in p(n) Schritten lösen kann.
Die Klasse P enthält alle Probleme bei denen dies auch mit einer deterministischen Turingmaschine möglich ist.

P schwere Probleme sind in der Praxis Algorithmen, welche ein akzeptables Laufzeitverhalten haben.
NP schwere Probleme sind in der Praxis meist nicht berechenbar. Sie haben ein Laufzeitverhalten in der Gegend von O(2^n) (also exponentiell wachsend mit der Eingabegröße).

Dies konnte aber nie bewiesen werden. Es steht immer noch aus ob P =/= NP.

## NP-Hardness
Wie zeigen wir, ob ein Problem NP-schwer ist?

- Polynomielle Reduzierung

%TODO: Definition mit <==> ausführen!

Falls A \\(\leq_p\\) B und B \\(\in\\) NP, dann ist auch A \\(\in\\) NP.

Falls A \\(\leq_p\\) B und B \\(\in\\) P, dann ist auch A \\(\in\\) P.

- Es stellt sich heraus, dass alle NP-schweren Probleme aufeinander reduzierbar sind.
- findet man einen effektiven Algorithmus für ein NP-schweres Problem, so findet man einen für alle NP-schweren Probleme. Dann wär P = NP
    - der Grund ist die transitivität der polynomiellen Reduzierung
- der Trick ist es, ein NP-Vollständiges Problem als initiales Problem zu finden.
    - anschließend können alle anderen Probleme auf dieses reduziert werden.
    - Dies ist das SAT Problem und wurde von Cook bewiesen (1971)

### SAT Problem
Das Erfüllbarkeitsproblem der Aussagenlogik (kurz SAT) ist das folgende.
Gegeben eine Aussagenlogische Formel F (beispielsweis a und b oder nicht c),
gibt es eine Belegung, welche diese Formel erfüllt.

### Typinferenz für FGJ ist NP-Schwer
- **Gegeben:** ein Java Program mit fehlenden Typannotationen.
- **Gefragt:** Gibt es eine Typeinsetzung für die fehlenden Annotationen, so dass ein korrektes Java Programm entsteht?

#### Reduktion auf SAT #### 

Wir zeigen, dass sich SAT auf _Typinferenz für Java_ reduzieren lässt.

% TODO: Wir müssen zeigen, dass die beiden Problem equivalent sind

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
    True sat(v1, v2, v3, o1, o2){
      return o1.or(v1, o2.and(v2, v3.not()));
    }
  }
```
