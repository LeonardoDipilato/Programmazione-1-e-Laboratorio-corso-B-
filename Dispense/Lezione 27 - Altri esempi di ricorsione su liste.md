# Lezione 27 (martedì 28 novembre 2017)
## Ricorsione su liste (parte 2)
### Ex.: Calcolo della somma degli elementi di una lista
```
let rec sum l =
    match l with
        [] -> 0
      | x::xs -> x + (sum xs);;
sum : int list -> int = <fun>
```

### Ex.#2: Cancellare l'ultimo elemento in una lista
***N.B.:*** come già detto, in Caml non è possibile modificare un valore costruito in precedenza. Quello che faremo sarà, infatti, scrivere una funzione che prenda una lista e restituisca la lista senza l'ultimo elemento.
```
let rec canc l =
    match l with
        [] -> []
      | x::[] -> []
      | x:xs -> x :: (canc xs);;
canc : 'a list -> 'a list = <fun>
```

### Ex.#3: Restituire l'ultimo elemento di una lista
```
# let rec last l =
    match l with
        x::[] -> x
      | x::xs -> last xs;;
last : 'a list -> 'a = <fun>
```

### Ex.#4: Calcolo del massimo elemento
```
# let rec max l =
    match l with
        x::[] -> x
      | x::xs -> if (x > max xs) then x
                                 else max xs;;
max : 'a list -> 'a = <fun>
```
Però chiamiamo due volte max nella stessa chiamata ricorsiva, perdendo tempo. Possiamo migliorare con:
```
# let rec max l =
    match l with
        x::[] -> x
      | x::xs -> let m = max xs
                 in
                    if (x > m) then x else m;;
max : 'a list -> 'a = <fun>
```

### Ex.#5: Max e min di una lista
Potremmo scrivere `min` e `max` come due funzioni ausiliarie e poi costruire `minimax` chiamando entrambe sulla lista, ma passeremmo attraverso l'intera lista due volte, perdendo tempo. Possiamo invece cercare entrambi contemporaneamente per migliorare la situazione:
```
# let rec minmax l =
    match l with
        x::[] -> (x, x)
      | x::xs -> let (p1, p2) = minmax xs
                 in
                    if (x < p1) then (x, p2)
                                   else if (x > p2)
                                        then (p1, x)
                                        else (p1, p2);;
minmax : 'a list -> 'a * 'a = <fun>
```

### Ex.#6: Invertire una lista (vers. 1)
```
# let rec reverse1 l =
    match l with
        [] -> []
      | x::xs -> (reverse1 xs)@[x];;
reverse1 : 'a list -> 'a list = <fun>
```
Il problema di questa funzione è che `@` per funzionare deve scorrere tutta la lista, e visto che viene chiamata molto spesso questo appesantisce l'esecuzione.

### Ex.#6: Invertire una lista (vers. 2)
Introduciamo il concetto di **accumulatore**. Un accumulatore è un parametro che passo alla funzione che conserverà il risultato fino a quel momento:
```
# let reverse2 l =
    let rec reverse_accum l1 acc =
        match l1 with
            [] -> acc
          | x::xs -> reverse_accum xs (x::acc)
    in
        reverse_accum l [];;
reverse2 : 'a list -> 'a list = <fun>
```
Paragoniamo ora reverse1 a reverse2:
```
# let rec crealista n =
    if n = 0 then [0]
             else n::(crealista (n - 1));;
crealista : int -> int list = <fun>

# let listalunga = crealista 10000;;

# reverse1 listalunga;;

# reverse2 listalunga;;
```
Notiamo la differenza nel tempo di esecuzione più che notevole.