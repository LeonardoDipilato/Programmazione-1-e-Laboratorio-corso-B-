# Lezione 12 (mercoledì 18 ottobre 2017)

## Albero di derivazione
Un ***albero*** è una struttura dati che è composto da nodi ed archi: un **nodo** è un oggetto che contiene dei dati, collegato tramite **archi** ad altri nodi detti **figli** (mentre il nodo stesso è detto padre). Ogni nodo ha un solo padre tranne la *radice*, che è l'unico nodo senza alcun padre. I nodi senza figli sono detti **foglie**.

Tramite gli alberi possiamo esprimere un linguaggio descritto da grammatiche: infatti è possibile costruire un ***albero di derivazione*** rispetto ad una grammatica G:

* La radice contiene la categoria sintattica iniziale
* Ogni nodo intermedio (ovvero che non è una radice né una foglia) contiene una categoria sintattica
* I figli di un nodo corrispondono ai simboli che si trovano a destra in una produzione per la categoria sintattica del nodo stesso
* Le foglie contengono simboli dell'alfabeto.

Prendiamo come grammatica di esempio, S -> ab | aSb. L'albero da costruirsi sarà così fatto:
```
        S
      / | \
    a   S   b
      / | \
    a   S   b
       / \
      a   b
```
Leggendo le foglie da sinistra a destra ottengo una parola del linguaggio (aaabbb).

Ex.: linguaggio delle parentesi bilanciate L<sub>P</sub> [Σ = {(, )}]

Data una equazione, ignoro tutti i numeri e gli operandi ottenendo qualcosa della forma (()()). Un'equazione si dice bilanciata quando per ogni parentesi chiusa, c'è n'è una aperta in precedenza e il numero delle parentesi aperte è uguale a quello di quelle chiuse.

Grammatica per L<sub>P</sub>: P -> () | (P) | PP

Anche se questa grammatica è corretta, abbiamo più di un modo di rappresentare ()()():
```
        P
      /   \
     P     P
    / \   / \
   () () (   ) = ()()()

        P
      /   \
     P     P
    / \   / \
   (   ) () () = ()()()
```
Quando una grammatica ammette più di un albero di derivazione per la stessa stringa, la grammatica si dice ***ambigua***.

Spesso è utile avere una grammatica non ambigua, ma non esiste un metodo generale per trasformare un grammatica ambigua in una non ambigua. DI SOLITO, il problema che causa ambiguità e la doppia ricorsione (ovvero, nel nostro caso, il fatto che P -> PP fosse un passaggio legale).

Per risolvere il problema, potremmo definire P -> () | (P) | AP, A -> () | (P)