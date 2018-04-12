# Lezione 31 (mercoled' 6 dicembre 2017)
## Operazioni su frame (continuo)
* Modifica di una associazione in un frame: se volessi riassegnare il valore di una variabile posso farlo tramite la funzione `mod` (che ha senso solo sui frame di memoria). Per effettuare la modifica, chiamiamo `mod(ν, l, v)` (che d'ora in poi scriveremo come ν[v/l]<sup>mod</sup>).

## Rappresentazione di una pila di frame
Ora che sappiamo rappresentare un singolo frame, ci interessa rappresentare una pila di frame. Per farlo, uso una lista che contiene i vari frame. Per comodità, si utilizzerà Ω per indicare la pila vuota. L'aggiunta in testa, invece, la si indicherà con un punto ., ad esempio φ<sub>1</sub>.φ<sub>0</sub>.Ω è la pila composta dai due frame φ.

## Operazioni su pile di frame
### Ricerca di un elemento nella pila
Data una generica pila π, indico con π(x)  il valore associato ad x nella pila π. Definiamo quindi π(x) = f(x) se π=f.π' e f(x) != BOTTOM | π'(x) se π=f.π' e f(x) = BOTTOM | BOTTOM se π = Ω.

### Modifica di una associazione in una pila
Data una pila π, π[b/a]<sup>mod</sup> = f[b/a]<sup>mod</sup>.π' se π=f.π' e f(a) != BOTTOM | f.π'[b/a]<sup>mod</sup> se π=f.π' e f(a)=BOTTOM | BOTTOM se π = Ω.

### Aggiunta di una associazione ad una pila
Assumo che la pila abbia almeno un frame, al più frame vuoto. In tal caso, π[b/a]<sup>add</sup> = f[b/a]<sup>add</sup>.π' se π=f.π' | BOTTOM se π=Ω.

## Sintassi del C
### Regole grammaticali
* Exp -> Num | Ide | Exp Op Exp
* Dec -> Type Ide; | Type Ide = Exp;
* Com -> Ide = Exp; |
    * if (Exp) Com else Com |
    * while (Exp) Com |
    * {Decl_list Com_list}
* Decl_list -> Dec | Dec Decl_list
* Com_list -> Com | Com Com_list

La semantica è data da come varia lo **stato** in base ai comandi nel frammento di programma che stiamo considerando.

## Definizione di stato di un programma
> Lo stato di un programma è una coppia (ρ, μ) dove ρ è una pila di ambiente e μ è una pila di memoria.

Si dice che ρ appartenga a P, l'insieme di tutte le possibili pile ambiente, e μ appartenga a M, l'insieme di tutte le possibili pile memoria. Sia P che M possono essere definiti ricorsivamente:

* P = {Ω} unito {φ.ρ' tale che φ : Ide -> Loc<sub>BOTTOM</sub>, ρ' appartente a P}

* M = {Ω} unito {ν.μ' tale che ν : Loc -> Val<sub>BOTTOM</sub>, μ' appartenente a M}