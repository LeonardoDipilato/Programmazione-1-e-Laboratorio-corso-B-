# Lezione 29 (venerdì 1 dicembre 2017)
## Funzioni di ordine superiore
Abbiamo introdotto le funzioni `forall`, `exists` e `map` che prendono come argomento una funzione e una lista compatibile con quella funzione. L'utilizzo di tali funzioni ci permette di scrivere funzioni che non fanno un uso *esplicito* della ricorsione (ovvero non hanno una chiamata a se stesse nel corpo e non necessitano, quindi, di `let rec` mentre un solo `let` è sufficiente).

### Ex.:
Supponiamo che, data una lista di coppie composte da una funzione e un valore, supponiamo di voler ottenere la lista composta da tutte le funzioni applicate ai propri valori.
```
# let f1 x = x + 1;;
# let f2 x = x + 2;;
# let l = [(f1, 10); (f2, 4); (F1, 8)];;

# let applicatutti l =
    let apply (f,x) = f x
    in
        map apply l;;
applicatutti : ('a -> 'b) * 'a list -> 'b list = <fun>

# applicatutti l;;
- : int list = [11;6;9]
```

## filter
La funzione `filter` data una lista e un predicato, restituisce la lista composta dagli elementi che soddisfano il predicato:
```
# let rec filter p l =
    match l with
        [] -> []
      | x::xs -> if p x then x::(filter p xs)
                        else filter p xs;;
filter : ('a -> bool) -> 'a list -> 'a list = <fun>
```
Esempio applicativo:
```
#let solopositivi l =
    let pos x = x >= 0
    in
        filter pos l;;
solopositivi : int list -> int list = <fun>
```

## foldr
Le funzioni di ordine superiore che abbiamo definito fino ad ora hanno il problema di lavorare su un solo elemento per volta e non è quindi possibile fare operazioni cumulative tipo il calcolare la lunghezza di una lista o la somma degli elementi. Introduciamo quindi la funzione `foldr` che utilizza un accumulatore per conservare le informazioni sul passo precedente.
```
# let rec foldr f a l
    match l with
        [] -> a
      | x::xs -> f x (folrd f a xs)
foldr : ('a -> 'b -> 'b) -> 'b -> 'a list -> 'b = <fun>
```
Dove a è il risultato del caso base. Esempi applicativi:
```
# let sum l =
    let f x y = x + y
    in
        foldr f 0 l;;
sum : int list -> int = <fun>

# let len l =
    let f x y = 1 + y
    in
        foldr f 0 l;;
len : 'a list -> int = <fun>
```

### Massimo di una lista
`max` è la funzione che cerca il massimo su una lista di valori solo non-negativi. Infatti, restituisce arbitrariamente 0 se la lista è vuota:
```
# let max l =
    let f x y = if x > y then x else y
    in
        foldr f 0 l;;
max : int list -> int = <fun>
```

`maxext`, invece, accetta anche liste negative, ma dà errore in caso di liste vuote:
```
# let maxext l =
    let f x y = if x > y then x else y
    in
        match l with
            x::xs -> foldr f x xs;;
```

### Somma degli elementi fino al primo zero
E.g. [5;3;6;0;1;2;0;2] = 5+3+6 = 14
```
let sommaprimadi0 l =
    let f x y = if x = 0 then 0 else x + y
    in
        foldr f 0 l;;
sommaprimadi0 : int list -> int = <fun>
```