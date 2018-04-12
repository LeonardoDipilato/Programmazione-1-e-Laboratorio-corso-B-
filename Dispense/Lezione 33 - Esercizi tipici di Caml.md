# Lezione 33 (giovedì 14 dicembre 2017)
## Esercitazione Caml per la prova intermedia
### Esercizio 3 della prova del 15/07/2015
> `twice : 'a -> 'a list -> bool`
> Tale che `twice x xs = true` se `x` occorre esattamente 2 volte in `xs`, `false` altrimenti.
```
# let twice x xs =
    let rec twice_acc e l acc =
        match l with
            [] -> if acc = 2 then true else false
          | f::fs -> if f = e then twice_acc e fs (acc+1)
                              else twice_acc e fs acc
    in
        twice_acc x xs 0;;
twice : 'a -> 'a list -> bool = <fun>
```

### Esercizio 2 del 16/12/2015
> `sumflat : int list list -> int list
> Data una lista di liste di interi fa la somma di tutti gli elementi di ogni lista
```
# let sumflat ll =
    let rec sumall l =
        match l with
            [] -> 0
          | x::xs -> x + sumall xs
    in
        map sumall ll;;
sumflat : int list list -> int list = <fun>
```

### Esercizio 3 seconda verifica intermedia 2016
> `canc : 'a list -> 'a -> 'a list`
> `canc` tale che `canc lis n` cancella l'ultima occorrenza di `n` in `lis`.
```
# let canc l n =
    let rec canc_acc l n conf toBeConf =
        match l with
            [] -> if toBeConf != [] then conf @ (tl toBeConf) else conf
          | x::xs -> if x = n then canc_acc xs n (conf @ toBeConf) (n::[])
                              else
                                if toBeConf = [] then canc_acc xs n (conf @ [x]) []
                                                 else canc_acc xs n conf (toBeConf @ [x])
    in
        canc_acc l n [] [];;
canc : 'a list -> 'a -> 'a list = <fun>
```

### Esercizio 4 del 10/01/2017
> `multiset : 'a list -> ('a * int) list`
> [3;3;5;5;5;6;7;7;6] -> [(3,2);(5,3);(6,1);(7,2);(6,1)]
```
# let multiset l =
    let rec multiset_acc l (curr, count) mult =
        match l with
            [] -> mult @ [(curr, count)]
          | x::xs -> if x = curr then multiset_acc xs (curr, count+1) mult
                                 else multiset_acc xs (x, 1) (mult @ [(curr, count)])
    in
        match l with
            [] -> []
          | x::xs -> multiset_acc xs (x, 1) [];;
multiset : 'a list -> ('a * int) list = <fun>
```