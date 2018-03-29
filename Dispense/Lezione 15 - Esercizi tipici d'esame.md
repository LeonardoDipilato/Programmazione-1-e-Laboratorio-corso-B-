# Lezione 15 (martedì 24 ottobre 2017)
## Esercizi tipici su linguaggi formali
1. Dato un linguaggio L, dimostrare se è regolare o meno e fornire una grammatica o un automa che lo definiscano
2. Data una grammatica G definire il linguaggio L generato e dimostrare se è regolare o meno.

Per la tipologia 1, se penso di poter costruire un automa, ci provo, altrimenti provo il Pumping Lemma. Se dovessi riuscire a costruire un automa, bene, ho dimostrato che è regolare. Se invece non ci riuscissi devo far ricorso al Pumping Lemma (contropositivo). Se fallisce (ovvero NOT P non è verificata), allora devo per forza provare con l'automa.

Ex.: L = {a<sup>n</sup>b<sup>m</sup>, m > n > 0}. Provo a tirar giù un automa, ma dato che devo per forza controllare prima le a delle b, sembrerebbe che io sia costretto ad avere infiniti stati per l'infinito numero di a che potrei avere per poi far nascere da ogni stato un equivalente numero di stati b con un loop finale. Questo mi suggerisce che L NON sia regolare. Provo quindi il Pumping Lemma.

Con un n arbitrario, scelgo una w che ha almeno n a. Così facendo, w sarebbe w=a<sup>n</sup>b<sup>n+1</sup>. Se chiamo xy = a<sup>n</sup>, avrei che x=a<sup>h</sup> (con 0 <= h < n), y = a<sup>n-h</sup>, z = b<sup>n+1</sup>. Ora, xy<sup>i</sup> = a<sup>h+i*(n-h)</sup>b<sup>n+1</sup>. Dato che n-h != 0, esiste i tale che i*(n-h)+h > n+1. Di conseguenza, il linguaggio non è regolare.

Per trovare una grammatica ci basta vedere qualche esempio per iniziare a notare qualche pattern: aa**abb**bbb, **abb**b, aaa**abb**bbb.

Possiamo quindi porre S -> abb | aSb | Sb. Questa funziona perché ogni volta che sostituiamo S stiamo aggiungendo più b che a (tranne con aSb, ma che per ricorsione prima o poi ricadrà in uno degli altri due casi dato che non è possibile avere una stringa infinita, in cui aggiungeremo almeno una b in più rispetto alle a) e perché ogni volta che sostituiamo S, S si trova in mezzo alla separazione delle a e delle b. Di conseguenza, aggiungendo a e b al lato giusto otteniamo una stringa legale.

Ex.#2: (IV appello 2017, 11 luglio) L = {a<sup>n</sup>b<sup>2n</sup>, n > 0, n % 3 == 1, m > 0}

Esempi di stringhe valide: aaaabb, aaaaaaabb, abb. Dato che n ed m sono indipendenti, c'è una buona probabilità che L sia regolare. Proviamo a definire un automa:
* Σ = {a, b}
* Q = {0, 1, 2, 3, 4}
* S = 0
* F = {4}
* δ = {<0, a, 1>,
       <1, a, 2>,
       <2, a, 0>, [***NOTA:*** questi tre stati risolvono il problema di n % 3 == 1]
       <1, b, 3>,
       <3, b, 4>,
       <4, b, 3>}

Dato che questo automa funziona, il linguaggio è regolare. Per soddisfare la richiesta di avere una grammatica regolare che lo descriva, traduco l'automa:

* 0 -> a1
* 1 -> a2 | b3
* 2 -> a0
* 3 -> b4 | b
* 4 -> b3

Questo metodo è meccanico e affidabile, ma volendo sarebbe possibile ridurre la quantità di definizioni nella grammatica prendendo direttamente la definizione di L. Infatti:

* S -> AB
* A -> a | aaaA
* B -> bb | bbB

## Esercizi per casa
Tutti i seguenti sono linguaggi su Σ = {a, b} di cui va verificata la regolarità o meno. Per tutti i linguaggi tranne il 6 va anche trovata una grammatica che lo definisce.

1. L = {ba<sup>k</sup>b<sup>k</sup>, k > 0}
2. L = {a<sup>k</sup>b<sup>k</sup>, k<=m} (con m fissato ad un valore arbitrario)
3. L = {a<sup>k</sup>b<sup>m</sup>, k,m>=z} (z > 0 fissato ad un valore arbitario)
4. L = {a<sup>k</sup>b<sup>m</sup>, k != m}
5. L = {a<sup>k</sup>b<sup>m</sup>}, (k % 2) != (m % 2) AND k,m >= 0}
6. L = {a<sup>k</sup>, k numero primo}

## Risoluzione
### Numero 1
Questo linguaggio, L = {ba<sup>k</sup>b<sup>k</sup>, k > 0}, è molto simile a a<sup>k</sup>b<sup>k</sup> il che ci dà un indizio circa la sua irregolarità. Proviamo, dunque, il Pumping Lemma.

Immaginiamo w = ba<sup>n</sup>b<sup>n</sup>. Possiamo spezzare w=xyz con x = ba<sup>s</sup> (con 0 <= s < n), y = a<sup>t</sup> (con 0 < t <= n-s), z = a<sup>n-t-s</sup>b<sup>n</sup>. In tal caso, xy<sup>i</sup>z non appartenente a L significa dire che esiste i tale che s+it+n-t-s != n. Sviluppando ulteriormente abbiamo che t(i-1) != 0 e dato che t != 0 allora i-1 != 0. Basta quindi una qualsiasi i diversa da 1 per far valere le ipotesi del Pumping Lemma.

Abbiamo quindi dimostrato l'irregolarità di L. Per trovare una grammatica che lo definisca, possiamo fare:

* S -> bS'
* S' -> ab | aS'b

### Numero 2
Questo linguaggio potrebbe far pensare che, dato che dobbiamo tener conto di quante a abbiamo messo per poi mettere lo stesso numero di b, possa ricadere nell'irregolarità. Invece, il fatto che ci sia un limite masssimo suggerisce che ci sia un numero finito di stati. Immaginiamo il seguente ASFD:
* Σ = {a, b}
* Q = {0, 1, 2, ..., m, m+1, ..., 2m}
* S = 0
* F = 2m
* δ = {<i, a, i+1>, <k, b, f(k)>}, con 0 <= i < m, 0 < k <= 2m-1 e f(k) uguale a k + 1 se k > m altrimenti uguale a 2m+1-k

Per m che tende all'infinito, la ASFD smetterebbe di avere un numero finito di stati. Ma visto che m è un numero finito arbitrario, possiamo costruire un apposito automa con esattamente 2m+1 stati. Dato che esiste un ASFD, allora il linguaggio è regolare.

Per definire una grammatica potremmo provare a riadattare l'automa:

* 0 -> a1
* 1 -> a2 | b(2m+1-1)
* 2 -> a3 | b(2m+1-2)
* ...
* m -> b(2m+1-m)
* m+1 -> b(m+2)
* ...
* 2m-1 -> bz

### Numero 3
Il linguaggio da studiare, L = {a<sup>k</sup>b<sup>m</sup>, k,m>z>0}, questa volta ha un limite inferiore (z) e due variabili indipendenti fra di loro (k, m). Non è improbabile che il linguaggio sia regolare, perché è possibile immaginare di costruire z stati finiti e mettere un loop sull'ultimo per assicurarsi che k>z e fare lo stesso per m. Proviamo quindi a costruire un ASFD con quest'idea in mente:

* Σ = {a, b}
* Q = {0, 1, ..., z, z+1, ..., 2z}
* S = 0
* F = {2z}
* δ = {<i, a, i+1>, [dove 0 <= i < z]
       <z, a, z>,
       <s, b, s+1>, [dove z <= s < 2z]
       <2z, b, 2z>}

Il linguaggio è dunque regolare.

Per costruire una grammatica potrei tradurre l'ASFD ottenuto, oppure pensando alla definizione:

S -> AIB
A -> a<sup>z</sup>
B -> b<sup>z</sup>
I -> aI | Ib | ε

S si assicura che ci siano almeno a<sup>z</sup> e b<sup>z</sup>, mentre I permette l'inserimento di a e b in maniera libera al posto giusto, eventualmente nullo.

### Numero 4
Il linguaggio in questione, L = {a<sup>k</sup>b<sup>m</sup>, k != m}, richiede di sapere quante a siano state messe per assicurarsi di non metterci lo stesso numero di b. Dato che k può essere arbitrariamente grande, dobbiamo per forza ricorrere ad un loop in un ASF per tenerne conto, e di conseguenza perdiamo l'informazione su quante a siano state messe esattamente. Sospettiamo, dunque, che L sia irregolare. E Pumping Lemma sia:

L'idea è di lavorare su un w ancora non finito nella forma a<sup>n</sup>b<sup>f(n)</sup>, dove dobbiamo ancora trovare la funzione f. Se sviluppiamo il lemma da qui abbiamo che x = a<sup>t</sup> (con 0 <= t < n), y = a<sup>s</sup> (con 0 < s <= n-t) e z = a<sup>n-t-s</sup>b<sup>f(n)</sup>. Se andiamo al passaggio successivo, otteniamo che per dimostrare che L sia irregolare dobbiamo poter dire che esiste i tale che t + is + n - t - s == f(n). Sviluppando otteniamo -> i = (f(n) + s - n)/s = (f(n) - n)/s - 1. Dato qui ricaviamo che:
1. f(n) > n (era anche abbastanza intuitivo dall'inizio, ma qui ne abbiamo la dimostrazione)
2. f(n) - n deve essere divisibile per s

Giocheremo sul punto due, ma questo significa semplicemente che, visto che s può essere un qualsiasi numero fra 1 e n, che f(n) - n deve essere multiplo di tutti questi. Si potrebbe dire che f(n)-n = n! (dato che n! è il prodotto di tutti i numeri minori o uguali a n). Da qui otteniamo che f(n) = n! + n, che è una funzione pura di n. Abbiamo quindi dimostrato che L è irregolare.

Per trovare una grammatica che lo definisca possiamo pensare a qualcosa che dia due possibilità:
1. Aggiungere lo stesso numero di a e b ma lasciare in sospeso una risoluzione, in maniera tale da non poterlo utilizzare per chiudere una stringa;
2. Aggiungere un numero arbitrario o di a o di b, ma rendendo impossibile l'aggiunta di entrambe.

In questo caso, il punto 1 funge da "equalizzatore" (tiene sempre equilibrate le due sezioni) e il secondo punto da "destabilizzatore" (che sposta l'ago della bilancia, di una quantità arbitraria) verso uno dei due estremi. Possiamo quindi pensare a:

S -> aSb | A | B
A -> aA | a
B -> Bb | b

### Numero 5
L = {a<sup>k</sup>b<sup>m</sup>}, (k % 2) != (m % 2) AND k,m >= 0}

La prima cosa da fare, in questo caso, è analizzare il "dominio" di k e m: detto in parole semplici, ciò che è scritto lì è che k ed m devono essere maggiori di zero e non entrambi pari o dispari. Abbiamo imparato che siamo capaci di tener conto di alcuni dati finiti circa le nostre variabili, quindi potremmo pensare di costruire un ASFD che risolva il problema. Prendiamo un problema per volta:
1. Dobbiamo avere almeno una a: questo significa che nello stato di partenza dovremmo evitare di avere una trasformazione per b.
2. Dobbiamo tener conto di se a sia pari o dispari: questo è semplice da eseguire tramite un loop di due stati. Inizialmente si potrebbe pensare di andare in loop fra gli stati 0 e 1, ma dato che da 0 non possiamo uscire tramite b per il punto 1 dobbiamo per forza introdurre uno stato 2. D'ora in poi, quindi, quando saremo in 1 significherà che abbiamo un numero dispari di a e quando saremo in 2 significherà che avremo un numero pari di a.
3. Dobbiamo relazionare b ad a: infatti, non possiamo conservare l'informazione sulla parità di a e analizzare quella di b per poi risolvere il tutto. Quindi significa che uscire dallo stato 1 tramite b o uscirci dallo stato 2 deve portare a due stati diversi, su cui poi andare in loop per trovare la "parità relativa" di b.

Costruiamo quindi:
* Σ = {a, b}
* Q = {0, 1, 2, 3, 4}
* S = 0
* F = 4
* δ = {<0, a, 1>,
       <1, a, 2>,
       <2, a, 1>,
       <1, b, 3>,
       <2, b, 4>,
       <3, b, 4>,
       <4, b, 3>}

Dato che abbiamo un ASFD, L è regolare.

Dobbiamo ora trovare una grammatica per descrivere L. L'idea sarebbe di utilizzare, come nell'esercizio 4, un equalizzatore e un destabilizzatore. L'equalizzatore si assicurerà che il numero di a e il numero di b abbiano la stessa parità, mentre il destabilizzatore si occuperà di variare arbitrariamente i due, concentrandosi su solo uno dei due, assicurandosi che non possano più avere la stessa parità. Proviamo quindi:

* S -> aSb | aAb | aBb
* A -> aaA | a
* B -> Bbb | b