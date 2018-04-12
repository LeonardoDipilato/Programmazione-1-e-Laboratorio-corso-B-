# Lezione 32 (martedì 12 dicembre 2017)
## Semantica dei frammenti di programmi C
Per capire la semantica possiamo farci guidare dalla sintassi:
* definiamo una funzione per ogni categoria sintattica
* ogni funzione è definita per casi, con un caso per ogni produzione per la categoria sintattica

## Semantica delle espressioni (Exp)
Sem<sub>e</sub> : Exp x P x M -> Val<sub>BOTTOM</sub>, dove Exp è l'espressione da valutare, P x M compone lo stato considerato mentre Val<sub>BOTTOM</sub> è il risultato dell'espressione.

Ex.: Sem<sub>e</sub>(x+2, [<x, l<sub>0</sub>>], [<l<sub>0</sub>, 4>]) = 6

Per definire Sem<sub>e</sub>, definiamo i 3 possibili casi di Exp:
* Num (n): Sem<sub>e</sub>(n, ρ, μ) = n [ignorando ρ e μ visto che non sono necessari per valutare n]
* Ide (x): Sem<sub>e</sub>(x, ρ, μ) = μ(ρ(x))
* Exp (e<sub>1</sub>) Op (op) Exp (e<sub>2</sub>): Sem<sub>e</sub>(e<sub>1</sub> op e<sub>2</sub>, ρ, μ) = Sem<sub>e</sub>(e<sub>1</sub>, ρ, μ) op Sem<sub>e</sub>(e<sub>2</sub>, ρ, μ)

## Semantica delle dichiarazioni (Dec)
Sem<sub>d</sub> : Dec x P x M -> P x M

Supponiamo di avere una funzione succloc : M -> Loc che restituisce la prima locazione libera in una data memoria M.

Definiamo, quindi, Sem<sub>d</sub> per casi:
* Type (T) Ide (x): Sem<sub>d</sub>(T x, ρ, μ) = (ρ[succloc(μ)/x]<sup>add</sup>, μ[?/e]<sup>add</sup>), dove ? è il valore che era presente in succloc(μ) prima di eseguire il tutto
* Type (T) Ide (x) = Exp(e);: Sem<sub>d</sub>(T x = e;, ρ, μ) = (ρ[succloc(μ)/x]<sup>add</sup>, μ[Sem<sub>e</sub>(e, ρ, μ)/e]<sup>add</sup>)

## Semantica dei comandi (Com)
Sem<sub>c</sub> : Com x P x M -> M

Definiamo Sem<sub>c</sub> per casi:
* Ide (x) = Exp (e);: Sem<sub>c</sub>(x = e;, ρ, μ) = μ[Sem<sub>e</sub>(e, ρ, μ)/ρ(x)]<sup>mod</sup>
* if (Exp (e)) Com (c<sub>1</sub>) else c<sub>2</sub>, ρ, μ): Sem<sub>c</sub>(c<sub>1</sub>, ρ, μ) se Sem<sub>e</sub>(e, ρ, μ) != 0 | Sem<sub>c</sub>(c<sub>2</sub>, ρ, μ) se Sem<sub>e</sub>(e, ρ, μ) = 0
* while (Exp (e)) Com (c): Sem<sub>c</sub>(while (e) c, ρ, μ) = Sem<sub>c</sub>(while (e) c, ρ, Sem<sub>c</sub>(c, ρ, μ)) se Sem<sub>c</sub>(e, ρ, μ) != 0 | μ se Sem<sub>e</sub>(e, ρ, μ) = 0
* {Dec_list (dl) Com_list (cl)}: Sem<sub>c</sub>({dl cd}, ρ, μ) = μ'

## Semantica delle dichiarazioni (Dec_list)
Sem<sub>dl</sub> : Dec_list x P x M -> P x M

Definiamo Sem<sub>dl</sub> per casi:
* Dec (δ): Sem<sub>dl</sub>(δ, ρ, μ) = Sem<sub>δ</sub>(δ, ρ, μ)
* Dec (δ) Dec_list (dl) = Sem<sub>dl</sub>(δ dl, ρ, μ) = Sem<sub>dl</sub>(dl, Sem<sub>δ</sub>(δ, ρ, μ))

## Semantica delle liste di comandi (Com_list)
Sem<sub>cl</sub> : Com_list x P x M -> M

Definiamo:
* Com (c): Sem<sub>cl</sub>(c, ρ, μ) = Sem<sub>c</sub>(c, ρ, μ)
* Com (c) Com_list (cl): Sem<sub>cl</sub>(c cl, ρ, μ) = Sem<sub>cl</sub>(cl, ρ, Sem<sub>c</sub>(c, ρ, μ))

## Semantica dei puntatori
Estendiamo le Exp e i Com con i puntatori:
* Exp -> ... | \*Ide | &Ide
* Com -> ... | \*Ide = Exp;

Estendiamo anche le funzioni semantiche aggiungendo nuovi casi:
* \*Ide (\*x): Sem<sub>l</sub>(*x, ρ, μ) = μ(μ(ρ(x)))
* &Ide (&x): Sem<sub>l</sub>(&x, ρ, μ) = ρ(x)
* \*Ide (*x) = Exp (e);: Sem<sub>c</sub>(\*x = e;, ρ, μ) = μ[Sem<sub>(e, ρ, μ)/μ(ρ(x))]<sup>mod</sup>