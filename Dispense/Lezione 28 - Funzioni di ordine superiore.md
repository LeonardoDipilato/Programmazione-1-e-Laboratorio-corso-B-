# Lezione 28 (mercoledì 29 novembre 2017)
## Altre funzioni su liste
### Prendere un numero arbitrario di valori dalla testa di una lista
```
# let rec take n l =
    match (n, l) with
        (0, l1) -> []
      | (n1, []) -> []
      | (n1, x::xs) -> x::(take (n1 - 1) xs);;
take : 'a list -> 'a list = <fun>
```
Abbiamo però un problema: che succede se n < 0? Nel pattern matching siamo nel terzo caso che restituirà sempre tutta la lista, mentre sarebbe più sensato se restituisse solo `[]`. Risolviamo con:
```
# let rec take n l =
    match (n, l) with
        (n1, l1) when n1 <= 0 -> []
      | (n1, []) -> []
      | (n1, x::xs) -> x::(take (n-1) xs);;
take : 'a list -> 'a list = <fun>
```

### Controllare che una lista sia (debolmente) crescente
```
# let rec ord l =
    match l with
        [] -> true
      | x::[] -> true
      | x::xs -> if (x <= hd xs)
                    then ord xs
                    else false;;
ord : 'a list -> bool = <fun>
```

## Esercizi per casa
### Esercizio 1
> Definire una funzione `drop` che data una lista ed un numero `n` restituistca la lista di tutti gli elementi esclusi i primi n.
```
# let rec drop n l =
    if n > 0 then 
        match l with
            [] -> []
          | x::xs -> drop (n-1) xs
    else l;;
drop : int -> 'a list -> 'a list = <fun>
```

### Esercizio 2
> Definire una funzione `ennesimo` che restituisca l'elemento in posizione `n` nella lista.
```
# let rec ennesimo n l =
    match n with
        0 -> hd l
      | x when x > 0 -> ennesimo (x-1) (tl l);;
```
***N.B.:*** molti casi non sono gestiti in quanto ho preferito decidere di restituire un errore piuttosto che restituire un valore di default in caso di errori di esecuzione (ovvero un indice negativo oppure un indice maggiore della lunghezza della lista). Inoltre, ho considerato la lista come zero-indexed.

### Esercizio 3
> Definire una funzione `sommacostante` che prende una lista di coppie e verifica che la somma degli elementi di ogni coppia sia sempre uguale per tutti gli elementi della lista. Si provi a risolvere senza utilzzare `fst` e `snd`.
```
# let rec sommacostante l =
    match l with
        [] -> true
      | (a,b)::xs -> let rec sommacostante_acc l acc =
                    match l with
                        [] -> true
                      | (a,b)::xs when a+b = acc -> sommacostante_acc xs acc
                      | other -> false
                 in
                    sommacostante_acc xs (a+b);;
sommacostante : int list -> bool = <fun>
```

## Funzioni di ordine superiore al primo
Le funzioni che prendono uno o più parametri "tipici" (int, float, liste, etc...) si dicono funzioni di primo ordine. Le funzioni che prendono altre funzioni come parametro si dicono di *ordine superiore*.

### La funzione forall
`forall` verifica che una condizione valga su tutti gli elementi di una lista. `forall` prende come parametri una funzione del tipo `'a -> bool` e una lista di tipo `'a list` e restituisce `true` se la funzione restituisce `true` su tutti gli elementi della lista e `false` altrimenti. Vediamo come costruire la funzione `forall`:
```
#let rec forall p l =
    match l with
        [] -> true
      | x::xs -> if not p x then false
                            else forall p xs;;
forall : ('a -> bool) -> 'a list -> bool
```
Vediamone anche qualche applicazione:
```
# let tuttipari l =
    let pari x = (x mod 2 = 0)
    in
        forall pari l;;

tuttipari : int list -> bool = <fun>

# let tuttipositivi l =
    let pos x = x > 0
    in
        forall pos l;;
tuttipositiv : int list -> bool = <fun>
```

### La funzione exists
`exists` verifica se esiste almeno un elemento nella lista che soddisfa un predicato p:
```
# let rec exists p l =
    match l with
        [] -> false
      | x::xs -> if p x then true
                        else exists p xs;;
exists : ('a -> bool) -> 'a list -> bool = <fun>
```

### La funzione map
`map` applica una funzione ricevuta come argomento a tutti gli elementi della lista:
```
# let rec map f l =
    match l with
        [] -> []
      | x::xs -> (f x)::(map f xs);;
map : ('a -> 'b) -> 'a list -> 'b list = <fun>
```
Esempio applicativo:
```
# let incrementa l =
    let f x = x + 1
    in
        map f l;;
incrementa : int list -> int list = <fun>
```