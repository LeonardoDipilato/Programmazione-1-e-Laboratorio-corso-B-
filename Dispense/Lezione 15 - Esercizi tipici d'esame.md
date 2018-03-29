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
Questo esercizio è molto simile a a<sup>k</sup>b<sup>k</sup>, il che ci dà un indizio circa la sua irregolarità. Proviamo, dunque, il Pumping Lemma.

* "Per qualsiasi n": immaginiamo n arbitrario.
* "Esiste w appartenente a L, con |w| >= n": prendiamo w = ba<sup>n</sup>b<sup>n</sup> che è lunga 2n+1>=w
* "Tale che se suddivido w in xyz, con |xy| <= n e y |= ε": scriviamo x = b<sup>g</sup>a<sup>h</sup> (dove 0 <= h < n-1 e g = 0 se n = 1, mentre 1 se n != 1), y = b<sup>1-g</sup>a<sup>t</sup> (dove 0 < t <= n-h) e z = a<sup>k-t-g-h</sup>b<sup>k</sup>.
* "Esiste una i tale che xy<sup>i</sup>z non appartiene a L": notiamo che se n = 1, abbiamo xy<sup>i</sup>z = b<sup>i*(1-g)</sup>a<sup>k</sup>b<sup>k</sup> = b<sup>i</sup>a<sup>k</sup>b<sup>k</sup>, dato che per n = 1, g = 0. Se n > 1, invece, abbiamo ba<sup>h+i*t</sup>a<sup>k-t-h-1</sup>b<sup>k</sup>. Per i = 0 abbiamo una stringa che non appartiene a L perché ba<sup>k-t-1</sup>b<sup>k</sup>, con t = 1 abbiamo ba<sup>k-2</sup>b<sup>k</sup>.

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
* 2m-1 -> b