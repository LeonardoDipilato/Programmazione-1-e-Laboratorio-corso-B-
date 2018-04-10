#Lezione 24 (martedì 21 novembre 2017)
## Progettazione di funzioni ricorsive
Il teorema di ricorsione consente di calcolare il grafico di una funzione ricorsiva, e il grafico consente di verificare se la funzione è **totale** (ovvero se per tutti gli input essa termina sempre). Un altro metodo per assicurarsi che una funzione sia totale è basato sul concetto di *relazione di precedenza indotta dalla funzione*

> Data una definizione ricorsiva di una funzione f : A -> A, la **relazione di precedenza indotta** dalla definizione di f è la relazione di ***f-precedenza***. Si dice che, dati x e y appartenenti ad A, x f-precede y se f(y) richiama ricorsivamente f(x). [***N.B.:*** spesso si userà semplicemente "precede" invece che "f-precede" quando è evidente quale sia la funzione alla quale facciamo riferimento]

**Ex.:** sia data f(n) = 0 se n = 0 | 1 se n = 1 | f(n-2) altrimenti, con f : N -> N (dove N sono i naturali). In tal caso, abbiamo la seguente relazione di precedenza: x precede y se x = y - 2 con y > 1. Questo perché, per calcolare f(0) e f(1) sono i casi base e quindi non fanno uso della ricorsione, mentre f(2) richiama 0 (che è uguale a 2 - 2), f(3) richiama 1 (3 - 2), f(4) richiama 2 (4 - 2), etc...

Provo, dunque, a disegnare una sorta di "totem" per capire chi precede chi, scrivendo:
```
y
x
```
dove x precede y. In tal caso, per la nostra funzione, otteniamo che:
```
.   .
.   .
4   5
2   3
0   1
```
Possiamo quindi notare che tutti i numeri dispari finiscono nel "totem" di 1 (ed hanno quindi un caso base a cui fare riferimento, terminando sicuramente), mentre tutti i dispari finiscono in 0. Questo significa che per qualsiasi n appartenente ad N, f(n) termina.

**Ex.#2:** f(n) = 0 se n = 1 | 1 + f(n+1) se n != 1, f : N -> N. Graficamente:
```
0   2
1   3
    4
    5
    .
    .
```

Si nota che 1 precede 0 e 1 è un caso base, quindi sia 0 che 1 terminano. Di contro, 2 non termina in quanto nella sua chiamata richiede 3 che a sua volta richiede 4 e così via. Indi per cui, nessun altro numero termina f(n).

**Ex.#3:** (successione di Fibonacci) fib(n) = 0 se n = 0 | 1 se n = 1 | fib(n-1) + fib(n-2) altrimenti. Graficamente:
```
.   .
.   .
.   5
  / |
4   |
| \ |
|   3
| / |
2   |
| \ |
0   1
```
Vediamo, quindi, che fib è totale, visto che tutti i valori sono fib-preceduti dal numero più piccolo di sé di 1 o di 2 e, dato che 0 e 1 sono casi base, 2 ha 0 e 1 come precessori, 3 ha 2 e 1, 4 ha 3 e 2 e così via.

## Programmazione ricorsiva e funzionale in Caml Light
Lavorare in Caml consiste nel valutare espressioni fornite al prompt "#" (espressioni rigorosamente terminate con ";;"). La risposta dell'interprete ad un'espressione è composto da tre parti:
1. Nome definito (- in caso di nessun nome)
2. Tipo dell'espressione (e.g. int)
3. Valore (risultato calcolato)

### Ex.:
```
# 33;;
- : int = 33

# 10 + 12;;
- : int = 22

# 3.4;;
- : float = 3.4
```

## Inferenza di tipi
Caml **inferisce** (deduce) il tipo dell'espressione dai valori e dalle operazioni in essa contenute. Ad esempio, `3+2` ha tipo int perché è una somma di int.

Il linguaggio si dice ***fortemente tipato***, ovvero *ogni* espressione ha un tipo ben definito e le operazioni devono essere fatte rispettando i tipi degli operandi.

Le operazioni aritmetiche esistono in due versioni diverse: tra interi (int) e tra frazionari (float):
* Su interi: +, -, *, /, mod
* Su frazionari: +., -., *., /.

### Ex.:
```
3.4 +. 6.2;;
- : float = 9.6

# 3.4 + 6.2
```
Quest'ultimo comando restituisce un errore di tipo.

## Bool
Possiamo anche scrivere espressioni logiche tramite il tipo **bool**, composto dai valori **true** e **false**. Abbiamo quindi:
* Operazioni di confronto: <, >, = [diverso dal == di C], !=, >=, etc...
* Operatori logici: &&, ||, not [diverso dal ! di C]

### Ex.:
```
# true;;
- : bool = true

# 3 < 5 && 1 = 2;;
- : bool = false

# not true;;
- : bool = false
```

## Associazione di espressioni a nomi
Possiamo dare un nome ad alcune espressioni tramite l'operatore `let`:
```
# let x = 10 + 2;;
x : int = 12
```
Si noti, però, la differenza fra *variabili* di C e *nomi* di Caml: infatti, mentre i valori salvati nelle variabili di C potevano essere modificato (da qui variabili), quello che facciamo in Caml è semplicemente dare un nome ad un'espressione. Questo ci permette di dare un nome ad oggetti altrimenti scomodi da scrivere oppure per rendere più chiaro alla lettura il tutto, ma non aggiunge alcuna altra funzionalità.

Esiste una piccola "scappatoia" al non poter modificare il valore associato ad un nome: la riassociazione. Infatti, è possibile assegnare un nuovo valore ad un nome tramite `let`, ma non è possibile modificare il valore all'interno di un'espressione (rendendo quindi impossibili i cicli come li conosciamo).

## Definire funzioni
L'operatore let può essere usato anche per definire funzioni. Se questo sembra strano è perché nei linguaggi funzionali (come Caml) le funzioni sono considerate dei **valori**.

### Ex.:
```
# let f x = x + 1;;
f : int -> int = <fun>
```
`f` è il nome della funzione, `int -> int` è il tipo (inferito) e `<fun>` è il valore funzione. Ciò che abbiamo fatto è stato creare una funzione che prenda un int x come input e restituisca il valore di x più 1.

Potrebbero sorgere dei dubbi su come abbia fatto l'interprete ad inferire il tipo di f:

1. `let f x =` ci ha suggerito che f fosse una funzione di parametro x, infatti, altrimenti, avrei avuto un solo nome sulla sinistra.
2. `x+1` mi dice che `x` deve essere int (perché l'operatore `+` accetta solo int), il che ci dà `int ->`
3. Siccome `x+1` è di tipo int, il risultato della funzione deve essere un int, completando quindi `int -> int`

Quando definisco una funzione creo un'associazione nell'ambiente della forma `λx.(x+1)`, dove λ (lambda) rappresenta la funzione che dato x (`λx`) calcola `x+1`. Ora che abbiamo questa funzione possiamo fare:
```
# f 2;;
- : int = 3
```
Il che corrisponde a fare `λx.(x+1)` che si trasforma, prendendo 2 e sostituendolo in x in ogni sua apparizione nella parentesi in `2+1` che restituisce quindi 3 come desiderato.

## Funzioni con più di un parametro
Se volessi sommare due numeri posso approcciare il problema in due modi, principalmente:
### Espressione **COPPIA**
Tramite l'operatore `,` (virgola) possiamo costruire coppie di espressioni come, ad esempio:
```
# 3, 5;;
- : int*int = 3, 5
```
Posso poi accedere ai singoli elementi di una coppia tramite gli operatori `fst` e `snd` che restituiscono, rispettivamente, il primo e il secondo elemento di una data coppia.

Posso, inoltre, definire delle **ennuple** usando la `,` più volte. Vediamo un paio di esempi:
```
# 6, 4.2;;
- : int*float = 6, 4.2

# 3 + 2, 4 - 1;;
- : int*int = 5, 3

# fst (3, 1);; // Si noti che ci vuole la parentesi in quanto le funzioni hanno precedenza sulla virgola
- : int = 3

# snd (3, 1);;
- : int = 1

# p = (6.2, 5);;
p : float*int = 6.2, 5)

# fst p;;
- : float = 6.2

# 3+2, 6,4 +. 1.0, 9, 8.4;;
- : int*float*int*float = 5, 7,4, 9, 8.4 
```
Tornando, quindi, all'obiettivo iniziale, possiamo definire:
```
# let somma (x, y) = x + y;;
somma : int*int -> int = <fun>

# somma (3, 2);;
- : int = 5
```
