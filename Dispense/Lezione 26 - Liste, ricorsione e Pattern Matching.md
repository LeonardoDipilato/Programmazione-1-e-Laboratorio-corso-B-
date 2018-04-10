# Lezione 26 (venerdì 24 novembre 2017)
## Funzioni ausiliarie locali
### Ex.:
Ammettiamo di voler scrivere una funzione che calcoli il perimetro di un triangolo date le coordinate dei tre angoli. Potremmo pensare di scrivere una funzione che somma le distanze tra gli angoli, ma per farlo dovremmo definire una funzione `dist` che calcoli la distanza. Se non volessimo inserire questa funzione nel corpo principale del programma (perché, ad esempio, non vogliamo che sia chiamata dall'utente senza passare per altre funzioni), possiamo scriverla all'interno della funzione `perimetro` per far sì che sia disponibile solo lì dentro:
```
# let perimetro p1 p2 p3 =
    let dist (x1, y1) (x2, y2) =
        sqrt ((x2 -. x1) *. (x2 -. x1) +.
              (y2 -. y1) *. (y2 -. y1))
    in
        (dist p1 p2) +.
        (dist p2 p3) +.
        (dist p3 p1);;

perimetro : float * float -> float * float -> float * float -> float = <fun>
```

### Ex.#2:
Ammettiamo di voler scrivere una funzione che prenda un float `x` e un bool `y` come input e restituisca l'area di un cerchio di raggio `x` se `y` è vera, altrimenti calcola l'area di un quadrato di lato `x`.
```
# let f x y =
    let quadrato z = z *. z
    in
        if y then
            let pi = 3.14
            in
                quadrato x *. pi
        else
            quadrato x;;

f : float -> bool -> float = <fun>
```

## Definizioni ricorsive
Data l'impossibilità di cicli "classici" in Caml, è necessario molto spesso appoggiarsi alla ricorsione per risolvere problemi. Proviamo, ad esempio, a scrivere una funzione `fact` che calcoli il fattoriale di un numero.
```
# let rec fact n =
    if n = 0 then 1
    else n * fact(n-1);;

fact : int -> int = <fun>
```
Si noti il `rec` immediatamente dopo il `let`. Questa accortezza serve a dire a Caml che la funzione che definiamo deve essere ricorsiva, definendo una **pila di attivazione** sulla quale verranno poi effettuati i calcoli ricorsivi.

### Ex.: Funzione di Fibonacci
```
# let rec fibo n =
    if n = 0 then 0
    else if n = 1 then 1
         else fib(n-1) + fibo(n-2);;

fibo : int -> int = <fun>
```

## Esercizi per casa
Definire ***ricorsivamente*** le seguenti funzioni.

### Esercizio 1
> `pow x y` che calcola x<sup>y</sup>
```
# let rec pow x y =
    if y = 0 then 1
    else x * pow x (y - 1);;

pow : int -> int -> int = <fun>
```

### Esercizio 2
> `modulo x n` che calcola x mod n (x % n)
```
# let rec modulo x n =
    if x < n
        then if x < 0 then modulo (x + n) n
                       else x
        else modulo (x - n) n;;

modulo : int -> int -> int = <fun>
```

## Liste in Caml
Una lista è una sequenza omogenea di valori, necessariamente tutti dello stesso tipo, ma che è pre-implementata in Caml e non necessita (a differenza di C) di essere implementata dall'utente. Alcuni esempi di creazione di liste:
```
# [7; 3; 5];;
- : int list = [7;4;5]

# [3.4; 7.0; 8.2; 4.1];;
- : float list = [3.4;7.0;8.2;4.1]

# [true; false; true];;
- : bool list = [true;false;true]

# let f x = x + 1;;
f : int -> int = <fun>

# let g x = x - 1;;
g : int -> int = <fun>

# [f; g];;
- : int -> int list = [<fun>; <fun>]

# let list1 = [3; 4];;
list1 : int list = [3;4]

# let list2 = [4; 1];;
list2 : int list = [4;1]

# [list1; list2];;
- : int list list = [[3;4];[4;1]]

# [];;
- : 'a list = <fun>
```

## Operazioni sulle liste
* La funzione `hd` restituisce il primo elemento di una lista:
```
# hd;;
- : 'a list -> 'a = <fun>

# hd [3; 1; 4];;
- : int = 3
```
* La funzione `tl` restituisce la lista ottenuta rimuovendo il primo elemento:
```
# tl;;
- : 'a list -> 'a list = <fun>

# tl [3; 1; 4];;
- : int list = [1;4]
```
[Si noti, però, che né `hd` né `tl` sono definite su liste vuote e causano un errore in caso dovessero essere chiamate su di esse.]

* La funzione `::` (cons) aggiunge un elemento in testa ad una lista:
```
# 4 :: [7; 8];;
- : int list = [4;7;8]

# 4 :: [];;
- : int list = [4]
```

* la funzione `@` (append) concatena due liste dello stesso tipo:
```
# [3;4] @ [7; 8];;
- : int list = [3;4;7;8]
```

## Funzioni ricorsive su liste
### Ex.: Calcolo della lunghezza di una lista
La lunghezza di una lista è uguale alla lunghezza della coda (tail, `tl`) della lista + 1. Notando che la lunghezza di `[]` è 0 otteniamo la seguente funzione:
```
# let rec len l =
    if l = [] then '
    else 1 + len (tl l);;
len : 'a list -> int = <fun>
```

### Ex.#2: Somma degli elementi di una lista
La somma degli elementi di una lista è uguale al primo elemento più la somma degli elementi della coda. Di conseguenza:
```
let rec sum l =
    if l = [] then 0
    else (hd l) + (sum (tl l))
sum : int list -> int = <fun>
```

## Pattern Matching
`match` è un costrutto che consente di confrontare il risultato di un'espressione con una sequenza di casi possibili, aumentando le "direzioni" in cui muoversi dopo un controllo condizionale (rispetto all'`if` che dà solo due opzioni). Esempio d'uso:
```
# let negazione b =
    match b with
        true -> false
      | false -> true;;
negazione : bool -> bool = <fun>
```

Un valore soddisfa un pattern quando esiste un modo di instanziare le variabili contenute nel pattern che consenta di ottenere il valore con cui lo stiamo confrontando. Vediamo un paio di esempi: [a sinistra i pattern, al centro i valori, a destra se fa match o meno (talvolta con spiegazione annessa)]
```
true -> true -> OK
true -> false -> NO
3 -> 3 -> OK
3 -> 4 -> NO
(3, 4) -> (3, 4) -> OK
(3, 4) -> (4, 3) -> OK
(x, y) -> (8, 7) -> OK, con x = 8 e y = 7
(x, 7) -> (8, 7) -> OK, con x = 8
(x, 7) -> (8, 2) -> NO
[] -> [] -> OK
8 :: [] -> [8] -> OK
x :: [] -> [7] -> OK, x = 7
x :: [] -> [true] -> OK, x = true
x :: y :: [] -> [1] -> NO
```

Rivediamo quindi la funzione `len` usando il pattern matching:
```
# let rec len l =
    match l with
        [] -> 0
      | x :: xs -> 1 + (len xs);;
len : 'a list -> int
```
Dato che :: richiede un elemento a sinistra e una lista di quell'elemento sulla destra, l'unica maniera di fare pattern matching è di chiamare `xs` la coda di `l` e `x` la sua testa.