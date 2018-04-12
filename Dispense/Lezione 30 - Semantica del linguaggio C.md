# Lezione 30 (martedì 5 dicembra 2017)
## Altri esempi con foldr
### Calcolare il numero di occorrenze del valore massimo di una lista
E.g. [3;5;2;3;5;1] -> 2 [5]
```
let contamax l =
    let f x (m, c) =
        if x = m then (m, c+1)
                 else if x > m then (x, 1)
    in
        match l with
            z::zs -> let (max, cont) = foldr f (z, 1) zs
                     in
                        cont;;
contamax : int list -> int = <fun>
```

### Rimozione degli elementi ripetuti in una lista
E.g. [3;7;5;3;5;3] -> [3;7;5]
```
let noripetizioni l =
    let f x y =
        let p z = x=z
        in
            if (esists p y) then y else x::y
    in
        foldr f [] l;;
noripetizioni : 'a list -> 'a list = <fun>
```

## Semantica del linguaggio C
La **semantica** è, in parole semplici, il significato di un programma. Questo significato è dato da come il programma calcola il risultato, ovvero tramite la sequenza di cambiamenti dello stato. Quello che abbiamo fatto fino ad ora è stato descrivere a parole gli stati, frame e così via. Ora vogliamo invece passare a descrivere il tutto in termini matematici tramite delle funzioni.

## Rappresentazione di un frame
Ambiente e memoria sono pile di frame. Un frame dell'ambiente è una rapresentazione tabellare di un insieme di associazioni come coppie fra il nome della variabile e l'indirizzo nel quale è salvato il suo valore. Potremmo quindi descrivere un frame tramite una funzione che preso il nome della variabile ne restituisce l'indirizzo. Chiamiamo quindi `Ide` l'insieme di tutti i possibili nomi di variabili (*ide*ntificatori) e `Loc` l'insieme di tutte le possibili *loc*azioni di memoria. Un frame così descritto è dunque una **funzione parziale** del tipo `φ : Ide -> Loc`. Possiamo invece rendere la funzione `φ` una funzione totale tramite il simbolo BOTTOM. Se definiamo quindi LOC<sub>BOTTOM</sub> = LOC unito {BOTTOM}. A questo punto, possiamo definire φ(m) con gli m non definiti come BOTTOM.

Similmente possiamo rappresentare i frame di memoria collegando la locazione al valore. Chiamiamo μ la funzione che vad a Loc a Val<sub>BOTTOM</sub> che, anche stavolta, è una funzione totale.

## Operazioni sui frame
* Lettura: in un frame di ambiente φ voglio sapere qual è la locazione associata ad un dato identificatore. Per farlo, basta chiamare φ(x) per ottenenere la locazione l<sub>0</sub>. In maniera simile, μ(l<sub>0</sub>) restituisce il valore a quella locazione.
* Aggiunta di un'associazione: per aggiungere un'associazione dobbiamo ridefinire φ (che ricordiamo essere una funzione definita per elencazione) aggiungendo il caso voluto. Per farlo, usiamo una funzione `add` chiamata come `add(φ, x, l)` che aggiunge la coppia <x, l> alla funzione φ. D'ora in poi, scriveremo φ[l/x]<sup>add</sup>.
