# Lezione 10 (mercoledì 11 ottobre 2017)

## Automi a stati finiti
Σ = {a, b}
Q = {0, 1, 2}
S = 0
F = {2}
δ = {<0, a, 1>,
     <1, a, 1>,
     <1, b, 2>,
     <2, b, 2>}

Data una stringa α appartenente a Σ*, α appartiene ad L se e solo se posso percorrere un cammino dallo stato iniziale fino ad uno degli stati finali attraversando transizioni eticchettate con i simboli di α presi da sinistra a destra.

Ex.: costruiamo l'automa per il linguaggio delle stringhe su Σ = {a, b} che contengono almeno 2 a

**Notazione:** data una stringa α appartenente a Σ* e un simbolo s appartenente a Σ, denotiamo con |α|<sub>s</sub> il numero di occerrenze di s in α (e.g. |abbaabb|<sub>a</sub> == 3)

Quindi L = {α tali che (|α|<sub>a</sub>>= 2) AND (α appartiene a Σ*)}

Possiamo quindi immaginare di partire dallo stato 0 e dover arrivare allo stato 2 per far sì che α appartenza a L. Possiamo quindi costruire il seguente ASF:

* Σ = {a, b}
* Q = {0, 1, 2}
* S = 0
* F = {2}
* δ = {<0, a, 1>, <1, a, 2>, <2, a, 2>, <0, b, 0>, <1, b, 1>, <2, b, 2>}

Ex.#2: costruiamo l'automa che generi L tale che tutte le stringhe di L abbiano "ab" come sottostringa. Formalmente, L = {αabβ, α, β appartenenti a Σ*}.

Costruiamo quindi:

* Σ = {a, b}
* Q = {0, 1, 2}
* S = 0
* F = {2}
* δ = {<0, a, 1>, <1, b, 2>, <2, a, 2>, <2, b, 2>, <0, b, 0>, <1, a, 1>}

Ex.#3: Σ = {a, b, c}, L = {(ab)<sup>n</sup>c<sup>m</sup>, n > 0, m >= 0}

Costruiamo quindi:

* Σ = {a, b, c}
* Q = {0, 1, 2, 3}
* S = 0
* F = {2, 3}
* δ = {<0, a, 1>,
       <1, b, 2>,
       <2, c, 3>,
       <2, a, 1>}

## La funzione δ
Data la natura di δ, è facile pensare di poterla immaginare come una funzione: infatti, dato lo stato attuale e il simbolo da analizzare, restituisce lo stato finale (essenzialmente, una funzione di due variabili che restituisce un solo risultato).

Quando, per qualsiasi coppia stato-simbolo δ(stato, simbolo) ammette un risultato, essa si dice ***funzione totale***.

Se, invece, esistesse una coppia stato-simbolo tale che δ(stato, simbolo) non è definita, parleremo di ***funzione parziale***.

## Estensione della condizione di accettazione
Similmente all'esempio 2, ammettiamo di voler costruire un automa che accetti solo le αabbβ, invece che αabβ. Sarebbe comodo poter definire il seguente:

* Σ = {a, b}
* Q = {0, 1, 2, 3}
* S = 0
* F = {3}
* δ = {<0, a, 0>,
       <0, b, 0>,
       <0, a, 1>,
       <1, b, 2>,
       <2, b, 3>,
       <3, a, 3>,
       <3, b, 3>}

C'è, però, un problema di fondo con δ in questo caso: la funzione δ(0, a) e δ(0, b) ammettono due risultati (0 e 1).

Possiamo quindi estendere il concetto di accettazione di una stringa tramite la seguente: una stringa è accettabile per un ASF quando esiste un percorso che porta dallo stato iniziale ad uno stato finale che rispetta le regole definite da δ.

## Automi a stati finiti deterministici (ASFD)
Un ASF si dice deterministico se la relazione di transizione δ può essere espressa come una funzione (totale o parziale).

## Automi a stati finiti non deterministici (ASFND)
Un ASF si dice non deterministico quando la relazione di transizione δ può avere più di un valore associato alla stessa coppia di stato-simbolo.

Segue, immediatamente, che gli ASFD sono un sottoinsieme degli ASFND (semplicemente perché ogni ASFD è anche un ASFND, mentre prima abbiamo trovato un ASFND che non potesse avere una δ definita come funzione, e che quindi non appartiene agli ASFD). Si potrebbe quindi (spoiler: erroneamente) pensare che dato che esistono infiniti elementi che appartengono agli ASFND ma non agli ASFD allora potrebbero esistere linguaggi non definibili tramite ASFD ma solo da un ASFND?

La risposta, come suggerito, è "no": di conseguenza, ogni ASFND può essere tradotto in un ASFD equivalente (ovvero che descrivano lo stesso linguaggio).

## Traduzione di ASFND in ASFD equivalente
### Metodo dei sottoinsiemi
L'idea è di raccogliere gli stati in cui uno stato manda sotto forma di un insieme che li contiene, per poi gestire da lì le varie possibilità.

Ad esempio, dato:

* Σ = {a, b}
* Q = {0, 1, 2, 3}
* S = 0
* F = {3}
* δ = {<0, a, 0>,
       <0, a, 1>,
       <0, b, 0>,
       <1, b, 2>,
       <2, b, 3>,
       <3, a, 3>,
       <3, b, 3>}

Prendiamo lo stato di partenza, {0}, che dovrebbe svolgere il ruolo di 0 nell'ASFND. Andiamo per step:

* δ(0, a) non è definita perché restituirebbe sia 0 che 1. Allora definiamo δ'({0}, a) = {0, 1}.
* δ(0, b) = 0, quindi δ'({0}, b) = {0}.
* Ora, chi è δ'({0, 1}, a)? Il risultato è l'unione di δ'({0}, a) e δ'({1}, a). Ma δ(1, a) non esiste, di conseguenza δ'({1}, a) è l'insieme vuoto, quindi δ'({0, 1}, a) = {0, 1}.
* Similmente, δ'({0, 1}, b) = (Unione di δ'({0}, b) e δ'({1}, b)) = (Unione di {0} e {2}) = {0, 2}.
* Ripetiamo il procedimento per δ'({0, 2}, a) ottenendo {0, 1}.
* δ'({0, 2}, b) = {0, 3}
* δ'({0, 3}, a) = {0, 1, 3}
* δ'({0, 3}, b) = {0, 3}
* δ'({0, 1, 3}, a) = {0, 1, 3}
* δ'({0, 1, 3}, b) = {0, 2, 3}
* δ'({0, 2, 3}, a) = {0, 1, 3}
* δ'({0, 2, 3}, b) = {0, 3}

L'ASF ottenuto sarà quindi:
* Σ = {a, b}
* Q = {{0}, {0, 1}, {0, 2}, {0, 3}, {0, 1, 3}, {0, 2, 3}}
* S = {0}
* F = {{0, 3}, {0, 1, 3}, {0, 2, 3}}
* δ = {<{0}, a, {0, 1}>,
       <{0}, b, {0}>,
       <{0, 1}, a, {0, 1}>,
       <{0, 1}, b, {0, 2}>,
       <{0, 2}, a, {0, 1}>,
       <{0, 2}, b, {0, 3}>,
       <{0, 3}, a, {0, 1, 3}>,
       <{0, 3}, b, {0, 3}>,
       <{0, 1, 3}, a, {0, 1, 3}>,
       <{0, 1, 3}, b, {0, 2, 3}>,
       <{0, 2, 3}, a, {0, 1, 3}>,
       <{0, 2, 3}, b, {0, 3}>}

Che, come si può notare, è un ASFD (con funzione totale, per di più).