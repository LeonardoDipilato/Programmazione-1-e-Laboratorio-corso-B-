# Lezione 09 (martedì 10 ottobre 2017)

## Interpretazione di un programma
Quando scriviamo un programma e proviamo ad eseguirlo (magari con successo), ciò che succede è che il programma viene mandato in un compilatore che crea un file eseguibile che il computer può leggere (infatti, il computer non ha i mezzi per capire nativamente C, e ciò con cui lavora sono semplicemente sequenze di 0 e 1).

Il compilatore è il mezzo che è capace di leggere il C (o un linguaggio qualsiasi, ma parlerò di C per via delle analogie con il corso), che controlla che il codice sia sintatticamente corretto (anche se magari privo di utilizzo o significato) e che viene poi riscritto in maniera che il computer possa leggerlo.

Per fare ciò, è necessario che il compilatore abbia al suo interno un componente che, dato un programma, verifica se è sintatticamente corretto.

## Teoria dei linguaggi formali
La differenza tra un linguaggio naturale e un linguaggio formale è, fondamentalmente, l'utilizzatore: il principale usufruitore di un linguaggio naturale, infatti, è l'uomo che da sempre è abituato a parlare per mezze misure, figure retoriche e altri costrutti ambigui. Di contro, un computer non ha niente di diverso da "vero" o "falso", non esistono terze opzioni. In quanto tale, un linguaggio formale, pensato per essere usato da un computer, DEVE essere privo di ambiguità (oltre che, per favorire l'implementazione e l'uso, deve avere regole molto ben definite senza eccezioni, dove possibile). Per questo, un linguaggio naturale, quale l'italiano, non potrebbe mai essere il linguaggio di un computer: infatti l'italiano è pieno di regole grammaticali con altrettante eccezioni, rendendone l'uso complesso anche per l'uomo stesso che ha inventato il linguaggio. Inoltre, in italiano sono presenti frasi ambigue da cui non è possibile tirar fuori una verità assoluta ("Paolo guarda Mario con il binocolo" non rende chiaro se Paolo stia guardando Mario per mezzo del binocolo o se Paolo stia guardando Mario, possibilmente perplesso, che ha un bincolo).

Possiamo quindi definire un linguaggio come un insieme:

* Linguaggio naturale
    * Insieme di frasi costruite utilizzando parole provenienti da un dato dizionario (un dizionario è un insieme di tutte le parole possibili).
* Linguaggio formale
    * Insieme di stringhe costituite utilizzando simboli provenienti da un dato alfabeto (un alfabeto è un insieme di tutti i simboli utilizzabili).

## Come si definisce un linguaggio formale?
Un linguaggio può essere infinito (ovvero, è possibile che l'alfabeto generi un numero infinito di stringhe utilizzabili). Per definire un linguaggio abbiamo due mezzi:
* Approccio generativo (grammatica):
    * Il linguaggio è generato da un set di regole applicate ad un alfabeto, generando tutte le possibili stringhe.
* Approccio riconoscitivo (automist a stati finiti)
    * Data una stringa, si controlla se essa appartiene all'insieme delle possibili stringhe oppure no.

## Definizioni
* Alfabeto: insieme ***finito*** di simboli. Generalmente indicato come Σ = {a, b, c}.
* Stringa (di un dato alfabeto Σ): una sequenza finita di simboli appartenenti a Σ
    * Lunghezza di una stringa: il numero di simboli che compone una stringa (abc, ccba, ab sono, rispettivamente, di lunghezza 3, 4 e 2).
    * Stringa di lunghezza 0: generalmente indicata con ε, la stringa di lunghezza vuota NON appartiene a Σ.

Dato un alfabeto Σ, denotiamo con Σ* l'insieme di tutte e sole le stringhe su Σ di lunghezza finita. Si noti che, stavolta, ε appartiene a Σ*, indipendentemente da chi sia Σ.

## Notazione
Indicheremo con a<sup>n</sup> = aaa...a [con un totale di n a]. Segue immediatamente che a<sup>0</sup> = ε.

Useremo anche le parentesi (che immagineremo non essere simboli in Σ) per raggruppare più simboli in maniera da poter effettuare operazioni su di essi. Ad esempio, a<sup>2</sup>(ab)<sup>3</sup> = aaababab che, per inciso, è diverso da aaaaabbb che è invece uguale a a<sup>5</sup>b<sup>3</sup>.

## Definizione di linguaggio
Dato un alfabeto Σ, un linguaggio su Σ è un sottoinsieme di Σ*.

## Automi a stati finiti (ASF)
Strumento che, dato un alfabeto Σ e una stringa s appartenente a Σ, mi dice se s appartiene a un dato linguaggio L sottoinsieme di Σ* oppure no. In breve, risponde a "s appartiene ad L?".

Partiamo da un esempio: immaginiamo Σ = {a, b} e un automa estremamente semplice. 
1. Presa una stringa di input nello stato 1 la manda tutta tranne il primo simbolo allo stato 2 **se** il primo simbolo è una a;

2. Lo stato 2 manda la stringa (sempre eccetto la prima lettera) allo stato 3 se la prima lettera è una b, altrimenti, se è una a, la rimanda in se stesso;

3. Lo stato 3 manda la stringa in se stesso se la prima lettera è una b. Inoltre, lo stato 3 è anche uno **stato finale**.

Facciamo un paio di esempi di input su questo ASF.

* aaab: (1) aaab -> (2) abb -> (2) bb -> (3) b -> (3) ε. Dato che abbiamo ε in uno stato finale, la stringa è valida per il nostro ASF.

* aaa: (1) aaa -> (2) aa -> (2) a -> (2) ε. Dato che abbiamo ε in uno stato non finale, la stringa NON è valida.

* aabba: (1) aabba -> (2) abba -> (2) bba -> (3) ba -> (3) a. Dato che non esistono transizioni legali per a dallo stato 3, la stringa non è valida.

* bbb: (1) bba. Dato che non esistono transizioni legali per b dallo stato 1, la stringa non è valida.

Dato questo ASF possiamo quindi dire che definisce un linguaggio, ma che forma ha L (l'insieme che contiene tutte le stringhe valide)? L = {a<sup>n</sup>b<sup>m</sup>, con n,m > 0}

## Definizione formale di ASF
Un automa a stati finiti A è una quintupla A = (Σ, Q, S, F, δ), dove:
* Σ è l'alfabeto
* Q è l'insieme **finito** di stati
* S è lo stato iniziale, con S appartenente a Q
* F è l'insieme di stati finiti (ovvero, stati finali), con F appartenente a Q.
* δ è la relazione di transizione che spiega come ci si muove da uno stato ad un altro (δ è sottoinsieme di QxΣxQ, dove Q è il prodotto cartesiano).

### Esempi:
Dato Σ = {a, b}, e la macchina che manda la stringa da 0 a 1 se a, da 1 a 2 se b, da 2 a 1 se a, con b stato finale, abbiamo che:

* Σ = {a, b}
* Q = {0, 1, 2}
* S = 0
* F = {2}
* δ = {<0, a, 1>, <1, b, 2>, <2, a, 1>}

L generato da questa ASF è L = {(ab)<sup>n</sup>, n > 0}

