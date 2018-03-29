# Lezione 11 (venerdì 13 ottobre 2017)

## Altri esempi di ASFND -> ASFD
Ex.: automa che accetta tutti i numeri divisibili per 5 (con Σ = {0, ..., 9}), ossia accetta tutte le stringhe che finiscono per 0 o 5.

L'approccio ASFND è immediato (indicando con Σ una cifra qualsiasi):

* Σ = {0, ..., 9}
* Q = {0, 1}
* S = 0
* F = 1
* δ = {<0, n, 0>, <0, 0, 1>, <0, 5, 1>}

Per il metodo dei sottoinsiemi abbiamo che

* Σ = {0, ..., 9}
* Q = {{0}, {0, 1}}
* S = {0}
* F = {0, 1}
* δ' = {<{0}, Σ - {0, 5}, {0}>,
<{0}, {0, 5}, {0, 1}>,
<{0, 1}, Σ - {0, 5}, {0}>,
<{0, 1}, {0, 5}, {0, 1}>}

## Formalizzazione della traduzione
Dato A = (Σ Q, S, F, δ) ASFND, l'automa A' = (Σ, Q', {s}, F', δ') ASFD ottenuto tramite il metodo di costruzione dei sottoinsiemi è tale che:
* Q' sia sottoinsieme di P<sub>q</sub> [dove P<sub>q</sub> è l'insieme delle parti di Q, ovvero l'insieme di tutti i possibili sottoinsiemi di Q]
* {s} appartiene a Q'
* per q* appartenente a Q' e a appartenente a Σ, δ(q*, a) = {q tale che (δ(q', a) = q) AND (q' appartiene a q*)}
* F' = {q* tale che (q* appartiene a Q') AND (q* intersecato F è l'insieme vuoto)}

## Definire linguaggi tramite grammatiche
Il linguaggio definito da un ASF è l'insieme delle stringhe sull'alfabeto Σ che sono accettate dall'automa, ma, come detto in precedenza, l'uso di un ASF non è strettamente necessario per definire un linguaggio. Possiamo, infatti, utilizzare delle regole grammaticali che, dato Σ, generino tutte le stringhe possibili.

Facciamo un esempio:

* Σ = {a, b, c, d} [alfabeto]
* S -> X Y [una stringa è composta da un X e un Y (che ancora dobbiamo definire)]
* X -> a [regola che ci dice che a è una X]
* X -> b
* Y -> c
* Y -> d

Prendendo ora S, possiamo derivare una qualsiasi stringa, sostituendo, di volta in volta, tutte le possibili opzioni. Infatti (esempio) S -> X Y -> a Y -> ac, ma anche S -> X Y -> b Y -> bc.

Questa grammatica genera quindi il seguente linguaggio L = {ac, ad, bc, bd}

## Definizioni formali
Una grammatica è una quadrupla G = <Σ, V, S, P>, dove Σ è un alfabeto composto da simboli detti "simboli terminali" (in quanto non vengono sostituiti ulteriormente, V è un insieme finito (con intersezione nulla con Σ) di simboli detti "categorie sintattiche" (o "simboli non terminali, in quanto sono derivabili), S, che appartiene a V, è la categoria sintattica iniziale (ovvero il punto i partenza delle derivazioni) e P è un insieme finito di produzioni (regole grammaticali) che hanno la seguente forma: A -> α, con A appartenente a V e α appartenente a (Σ unito V)* (ovvero, tutte le possibili combinazioni finite dei simboli di Σ e V).

## Esempi
Ex.: G = <{a, b}, {S}, S, {S->ab, S->abS}> è una grammatica dove {a, b} è Σ, {S} è V, S è S stesso e {S->ab, S->abS} è P.

### Derivazioni
* S -> ab
* S -> abS -> abab
* S -> abS -> ababS -> ababab
* ...

Notiamo che il linguaggio generato è del tipo L = {(ab)<sup>n</sup>, n > 0}

Ex.#2: G = <{a, b}, {S}, S, {S -> ab, S -> aSb}>

### Derivazioni
* S -> ab
* S -> aSb -> aabb
* S -> aSb -> aaSbb -> aaabbb
* ...

Notiamo che il linguaggio generato è del tipo L = {a<sup>n</sup>b<sup>n</sup>, n > 0}. ***Nota:*** L, in questo caso, non è generabile da un ASF in quanto non è realizzabile tramite un numero finito di stati (visto che richiederebbe di "memorizzare" n che è un numero arbitrario per poi controllare di avere il giusto numero di b).

Quest'ultima nota ci dà uno spunto interessante: esistono grammatiche che definiscono linguaggi che gli ASF non definiscono, ma NON esistono linguaggi definiti da ASF non definibili tramite grammatiche. In breve, chiamando L<sub>ASF</sub> i linguaggi generati dalle ASF e L<sub>G</sub> i linguaggi generati da grammatiche, L<sub>ASF</sub> è un sottoinsieme di L<sub>G</sub>. Questo ci suggerisce, inoltre, che esista un metodo per convertire un automa in grammatica.

Ad esempio: dato un ASFD della forma
* Σ = {a, b}
* Q = {0, 1, 2}
* S = 0
* F = {2}
* δ = {<0, a, 1>,
       <1, a, 1>,
       <1, b, 2>,
       <2, b, 2>}

Generando L = {a<sup>n</sup>b<sup>m</sup>, n,m > 0}

Posso facilmente associare categorie sintattiche agli stati:

* S -> aA
* A -> aA
* A -> bB
* A -> b
* B -> bB
* B -> b

Che, derivando, mi genera lo stesso linguaggio L.

Posso, inoltre, semplificare la scrittura della grammatica raggruppando le produzioni per la stessa categoria sintattica, ottenendo:

* S -> aA
* A -> aA | bB | b
* B -> bB | b