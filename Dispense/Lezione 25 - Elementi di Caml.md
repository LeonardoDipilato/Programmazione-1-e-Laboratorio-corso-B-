# Lezione 25 (mercoledì 22 novembre 2017)
## Funzioni con più di un parametro (parte 2)
Il secondo modo per definire una funzione con più di un parametro è utilizzare una funzione che prenda come input il primo parametro e restituisca una funzione a cui dare in pasto il secondo parametro. Concretamente:
```
# let somma x y = x + y;;
somma : int -> int -> int = <fun>

# somma 3 5;;
- : int = 8
```
L'associazione che troviamo nello stato al nome somma sarà quindi `λx.λy.(x+y)` che viene valutato, chiamando `somma 3 5` come `λx(3).λy.(x+y)` => (\*) `λy.(3+y)` => `λy(5).(3+y)` => `3+5` => `8`.

Se ci fermiamo al passaggio (\*) scopriamo che dopo la prima valutazione abbiamo ottenuto un'espressione che sarebbe identica a ciò che otterremo definendo `let sommatre y = 3 + y`. Di conseguenza, è chiaro come la funzione `somma` sia di tipo `int -> (int -> int)` dove il primo int sarebbe la x che, passato all'interno della funzione mi genera una funzione che vada da int a int. Questo metodo di passaggio di argomenti viene detto "currying" dal nome del matematico che ha studiato il lambda calculus (che è la logica sulla quale si basano molti linguaggi funzionali).

## Applicazione parziale di una funzione
Come ci suggerisce lo sviluppo della funzione `somma`, è possibile fermarci ad uno stato intermedio fornendo solo un argomento, ricevendo come risultato una funzione. Potevamo, infatti, definire `sommatre` come `let sommatre = somma 3`. Infatti, dal tipo di somma (`int -> int -> int`) vediamo che, passando 3 come input otteniamo una funzione `int -> int` che fa esattamente ciò che vogliamo che `sommatre` faccia.

## Funzioni polimorfe
La funzione identità `let i x = x;;` ci dà poche informazioni sul suo tipo: infatti non ci sono operazioni che vincolano x ad essere di qualche tipo nello specifico in quanto tutto ciò che facciamo è restituire il medesimo argomento ricevuto. Se volessimo inferire il tipo di i otterremo che:
* i è una funzione con un parametro, e quindi sarà della forma `a -> a` dove a è il tipo di x.
* Però, come già detto, non sappiamo nulla su x in quanto qualsiasi input va bene. Per ciò, i si dice **funzione polimorfa** (ovvero che non abbia un tipo ben definito ma supporti una gamma di tipi).

Per esprimere quest'informazione, l'interprete usa una variabile di tipo. In tal caso, otterremmo che i sarà `i : 'a -> 'a = <fun>`. Questo ci dice che il tipo dell'argomento può essere qualunque (infatti non abbiamo limitazioni su `'a`), ma che l'output **deve** essere dello stesso tipo `'a`.

### Ex.:
```
# let maggiore x y = x > y;;
maggiore : 'a -> 'a -> bool = <fun>

# let negativo = maggiore 0;;
negativo : int -> bool = <fun>

#let first (x, y) = x;;
first : 'a*'b -> 'a = <fun>
```
L'ultimo esempio, nel particolare, è molto informativo: infatti, notiamo che `first` prende come input una coppia di tipi `'a` e `'b` (che potrebbero, ma non sono obbligati, ad essere diversi) e restituisce come output un valore di tipo `'a`.

## Passaggio di funzioni a funzioni
Abbiamo visto ceh in Caml le funzioni sono valori che possono essere associate ad un nume e memorizzate nell'ambiente assieme ai valori interi, float, etc..). Possiamo pensare, quindi, di passare una funzione ad un'altra come argomento di tale.

### Ex.:
```
#let apply f x = f x;;
apply : ('a -> 'b) -> 'a -> 'b = <fun>
```
La funzione apply prevede due parametri `f` ed `x`, dove `f` deve essere una funzione che vada dal tipo di `x`, chiamato `'a` in un tipo qualsiasi (anche lo stesso di `'a`, eventualmente), chiamato `'b`.

## Esercizi per casa
Definire le seguenti funzioni cercando di intuire il tipo che sarà inferito dall'interprete Caml

### Numero 1
> Funzione che prenda un intero e restituisca la coppia formata dal numero stesso e dal successivo
```
# let f x = (x, x+1);;
f : int -> (int*int) = <fun>
```

### Numero 2
> Funzione che prenda una coppia di valori di qualunque tipo (anche diversi tra loro) e restituisce la coppia in cui i due elementi hanno posizione scambiata
```
# let swap (x, y) = (y, x);;
swap : ('a, 'b) -> ('b, 'a) = <fun>
```

### Numero 3
> Funzione **double_apply** che prenda una funzione f e un argomento x e applica due volte la f ad x
```
# let double_apply f x = f (f x)
double_apply : ('a -> 'a) -> 'a -> 'a = <fun>
```

### Numero 4
> Funzione **pair_apply** che prenda una funzione f ed una coppia e restituisca la coppia ottenuta applicando f ad ognuno degli elementi della coppia ricevuta.
```
# let pair_apply f (x, y) = (f x, f y)
pair_apply : ('a -> 'b) -> ('a * 'a) -> ('b * 'b) = <fun>
```

## Definizione di funzioni per casi
### Valore assoluto
Ricordiamo la funzione abs(x) = x se x >= 0 | -x se x < 0. Uso un'espressione condizionale `if` della forma `if CONDIZIONE then ESPRESSIONE else ESPRESSIONE`.
```
# let abs x = if x >= 0 then x
                        else -x;;
abs : int -> int = <fun>
```
Notiamo l'inferenza di `x` come `int` in quanto `x >= 0` è possibile solo se `x` è del tipo `int`.

### Massimo tra due valori
```
# let max x y = if x > y then x
                         else y;;
max : 'a -> 'a -> 'a = <fun>
```

## Definizioni locali
Immaginiamo di voler scrivere areacerchio(r) = r<sup>2</sup>*π, dove π=3.14. Potremmo utilizzare una definizione locale (ovvero una definizione che esiste solo all'interno della funzione) per avere pi = 3.14:
```
# let pi = 3.14 in
    r *. r *. pi;;
areacerchio : float -> float = <fun>
```

Data l'alta flessibilità del concetto di valore (e, nel particolare, l'idea che le funzioni siano valori), è possibile utilizzare let per definire delle **funzioni ausiliarie** all'interno della funzione stessa. Ad esempio:
```
# let f x =
    let g z = 2 * z in
        (g x) + 1;;
f : int -> int = <fun>
```
Data la definizione locale di `g`, non sarà possibile chiamarla dall'esterno di `f`, mentre quest'ultima eseguirà senza problemi:
```
# f 10;;
- : int = 21

# g 4;;
```
ERRORE.

## Esercizi per casa
### Numero 5
> Funzione **non_neg** che prenda una funzione f e un valore x e restituisce il massimo tra f(x) e 0. Provare, inoltre, ad applicare parzialmente la funzione per vedere cosa si ottiene.
```
# let non_neg f x =
    let max x y = if x > y then x
                           else y
    in
        max (f x) x;;
non_neg : ('a -> 'a) -> 'a -> 'a = <fun>

# non_neg succ;;
- : int -> int = <fun>
```

### Numero 6
> Funzione **best** che prenda due funzioni f e g e un valore x e restituisca il massimo tra f(x) e g(x). Provare inoltre ad applicare parzialmente la funzione senza passare x per vedere cosa si ottiene.
```
# let best f g x =
    let max x y = if x > y then x
                           else y
    in
        max (f x) (g x);;
best : ('a -> 'b) -> ('a -> 'b) -> 'a -> 'b = <fun>

# best succ succ;;
- : int -> int = <fun>
```

### Numero 7
> Funzione **ordina** che prende una ennupla di 3 valori e restituisce la ennupla degli stessi tre valori messi in ordine crescente
```
# let ordina (x, y, z) =
    if x < y
        then if y < z
            then (x, y, z)
            else if x < z
                then (x, z, y)
                else (z, x, y)
        else if x < z
            then (y, x, z)
            else if y < z
                then (y, z, x)
                else (z, y, x);;
ordina : 'a * 'a * 'a -> 'a * 'a * 'a = <fun>
```

### Numero 8
> Funzione **soluzioni** che prenda tre valori a, b e c che rappresentano i coefficienti di una equazione di secondo grado e restituisca una coppia in cui il primo elemento è un bool che dice se l'equazione ha soluzioni (discriminante >= 0) e il secondo è a sua volta una coppia con le due soluzioni (possibilmente coincidenti) dell'equazione o con (0, 0) se non ci sono soluzioni reali.
```
# let soluzioni a b c =
    let discriminante =
        (b *. b) -. (4.0 *. a *. c)
    in
        if discriminante < 0.0 
            then (false, (0.0, 0.0))
            else (true, (((b -. (sqrt discriminante)) /. (2.0 *. a)), (b +. (sqrt discriminante)) /. (2.0 *. a)));;
soluzioni : float -> float -> float -> bool * (float * float) = <fun>
```