# Lezione 2 (lunedì 18 settembre)

## Comando condizionale (If)
***Sintassi:***
```
if (guardia o condizione)
    comando da eseguire se la condizione è vera
else
    comando da eseguire se la condizione è falsa
```
Il ramo *if* è ciò che definisce il comando condizionale ed è quindi necessario. Di contro, l'else è opzionale. La condizione (anche nota come guardia) deve essere un'espressione logica valutabile in **vero** o **falso**.

Ex. **Calcolo del massimo fra due numeri**:
```
int x = 10;
int y = 12;
int max;
if (x > y)
    max = x;
else
    max = y;
```
Dato che x è maggiore di y, l'affermazione `x > y` è vera. In tal caso, if eseguirà il suo blocco, assegnando a max il valore di x. Se y fosse, invece, stato 8, dato che `x > y` è falsa, questa volta verrà eseguito il codice dell'else.

Questo è un concetto molto importante della programmazione in quanto vediamo che effettivamente l'input può variare l'esecuzione di un programma.

## Espressioni logiche
Le espressioni logiche possono essere di diversi tipi:
* Espressioni di confronto
    * >, <, >=, <=, ==, !=
        * si presti particolare attenzione a `==` (uguaglianza, restituisce vero se i due valori che confronta sono uguali) che è diverso da `=` usato per l'assegnazione.
* Connettivi logici
    * && (AND): poste due espressioni logiche alla sua destra e sinistra restituisce vero se e solo se entrambe sono vere, mentre restituisce falso in tutti gli altri casi.
    * || (OR): poste due espressioni logiche alla sua destra e sinistra restituisce falso se e solo se entrambe sono false, mentre restituisce vero in tutti gli altri casi.
    * ! (NOT): posta un'espressione logica alla sua destra, restituisce falso se essa è vera, mentre restituisce vero se essa è falsa.

## Comando iterativo (ciclo)
***Sintassi:***
```
while (guardia o condizione)
    comando da eseguire
```
While funziona in maniera simile ad if nel senso che esegue il comando al suo interno quando la guardia è vera mentre non esegue nulla quando la guardia è falsa. Differisce, però, dal comando condizionale nel senso che una volta eseguito il programma torna al comando iterativo stesso. Se la guardia è ancora vera, il codice viene ripetuto (ad nauseam se la condizione rimane sempre vera), fermandosi solo se la condizione dovesse divenire falsa.

Per ciò, è importante utilizzare per i cicli guardie che non siano staticamente un valore, correndo il rischio di incorrere in un loop infinito.

* Ex. conto alla rovescia (ad ogni ciclo riduce x di uno finché x > 0):
```
int x = 5;
while (x > 0)
    x = x - 1
```
***N.B.***: il valore finale di x sarà 0 (e non 1 come qualcuno potrebbe aspettarsi). Questo perché quando x == 1, x > 0 e quindi il codice verrà eseguito, riducendo ulteriormente x di 1.

* Ex.#2 fattoriale di un numero:
```
int n = 5;
int f = 1;
while (n > 1) {
    f = f * n;
    n = n - 1;
}
```
Le due parentesi graffe permettono di eseguire più di un comando all'interno di un ciclo while (o di un'espressione if o qualsiasi comando che esegua un solo comando).

* Ex.#3 calcolo del massimo comun divisore con il metodo di euclide.
    * dati x e y, sottraggo il più piccolo al più grande e continuo finché i due numeri non diventano uguali.
```
int x = 30;
int y = 12;
int mcd;
while (x != y)
    if (x>y)
        x = x - y;
    else
        y = y - x
mcd = x
```

Chiamiamo iterazione **indeterminata** un ciclo che non so a priori quante volte venga ripetuto (ad esempio, il MCD). Chiamiamo invece iterazione **determinata** un ciclo che so quante volte verrà ripetuto (ad esmepio, per il fattoriale, il fattoriale di n richiederà n-1 iterazioni)

## Comando per iterazione determinata (for)
***Sintassi:***
```
for (comando di inizializzazione; guardia; comando di aggiornamento)
    comando da eseguire
```
Esempio: fattoriale 2.0
```
int n = 5;
int f = 1;
for (int i = 1; i <= n; i = i + 1)
    f = f * i;
```
Il ciclo for:
1. Esegue l'inizializzazione;
2. Valuta la condizione:
    * Se vera:
        1. Esegue il corpo;
        2. Esegue il comando di aggiornamento;
        3. Ricomincia da 2;
    * Se falsa, termina il ciclo for.

## Nota sui blocchi
Data l'utilità dei blocchi `{ }` per eseguire più di un comando tramite un unico comando condizionale o di iterazione, spesso capita che questi blocchi possano contenere una sequenza di comandi più o meno complessa. Una cosa a cui si deve fare attenzione è alla dichiarazione di variabili all'interno di un blocco. Infatti, in caso dovessimo dichiarare una variabile all'interno di un blocco questa risulterà utilizzabile all'interno del blocco stesso (e funge infatti da ottima variabile temporanea di cui poi disporre), ma smette di esistere una volta usciti dal blocco.

Ex.:
```
int x = 8;
if (x > 0) {
    int y = 2;
    x = x - y
} 
x = x + y;
```
Il comando `x = x - y` non causa problemi in quanto, all'interno del blocco, y esiste e vale 2 (al valore x verrà, infatti, assegnato il valore 6 al termine dell'if). Di contro, il comando `x = x + y` risulterà in un errore in quanto la variabile y non esiste più.

Ex.#2 date due variabili x e y, assegnare a x il valore del massimo dei due e ad y il minimo:
```
int x = 1;
int y = 14;
if (x < y) {
    int maxTemp = y;
    y = x;
    x = maxTemp;
}
```
Questo è un esempio di corretto utilizzo di dichiarazione di variabili in un blocco: dato che all'esterno del programma la variabile max non ha più ragione di esistere, la dichiariamo all'interno del blocco per far sì che termini di esistere una volta terminata la sua funzione.