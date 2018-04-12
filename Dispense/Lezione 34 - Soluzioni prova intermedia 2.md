# Lezione 34 (venerdì 15 dicembre 2017)
## Soluzione simulazione di seconda prova intermedia
### Esercizio 1
> `cancella : 'a list -> 'a list`
> Data una lista ne elimina gli ultimi due elementi. Se la lista contiene un solo elemento, questo viene eliminato
```
let rec cancella l =
    match l with
        [] -> []
      | x::[] -> []
      | x::(xs::[]) -> []
      | x::xs -> x :: cancella xs;;
cancella : 'a list -> 'a list = <fun>
```

### Esercizio 2
> `sposta : int -> 'a list -> 'a list`
> Dato un intero n e una lista, sposta i primi n elementi della lista in coda, disponendoli in ordine inverso
```
# let sposta n l =
    let rec sposta_acc n l acc =
        if n = 0 then (l @ acc)
                 else match l with
                          [] -> (l @ acc)
                        | x::xs -> sposta_acc (n-1) xs (x::acc)
    in
        sposta_acc n l [];;
```

### Esercizio 3
> `take : int -> 'a list -> 'a list`
> Dato un intero n e una lista resituisce i primi n elementi della lista ***senza ricorsione esplicita***
```
# let take n l =
    let length lis =
        let f x y = 1 + y
        in
            foldr f 0 lis
    in
        let f x (taken, acc) =
            if acc > 0 then (taken, acc-1)
                    else (x::taken, 0)
        in
            match foldr f ([], (length l) - n) l with
                (taken, y) -> taken;;
```

### Esercizio 4
> `cancellacoppie : (int * int) list -> (int * int) list`
> Data una lista di coppie elimina gli elementi della lista la cui somma degli elementi è 10 ***senza ricorsione esplicita***
```
# let cancellacoppie lis =
    let rec filter p l =
        match l with
            [] -> []
        | x::xs -> if p x then x::(filter p xs)
                            else filter p xs
    in
        let checkIfNot10 (a, b) = a+b!=10
        in
            filter checkIfNot10 lis;;
```