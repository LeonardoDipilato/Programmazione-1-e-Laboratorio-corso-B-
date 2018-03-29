# Lezione 13 (giovedì 19 ottobre 2017)

## Grammatica per i comandi del linguaggio C
Σ<sub>C</sub> = {a, b, c, ..., 0, 1, 2 ,..., =, !, <, >, {, }, ..., if, else, ...}

Com -> Ide = Exp; | if (Exp) Com else Com | while (Exp) Com | { SeqCom }
SeqCom -> Com | Com SeqCom
Ide -> Letter Seq
Letter -> a | b | c | ... | z
Cif -> 0 | ... | 9
Seq -> Letter | Cif | ε | LetterSeq | CifSeq
Exp -> Num | Exp + Exp | Exp * Exp | ... | (Exp)
Num -> Cif | CifNum

Abbiamo quindi G<sub>C</sub> = {Σ<sub>C</sub>, V<sub>C</sub>, Com, P<sub>C</sub>}, dove V<sub>C</sub> = {Com, Exp, ...} e P<sub>C</sub> = { tutta la roba scritta sopra }

Ora, data 3*2+5 ci chiediamo, è un'espressione corretta?

        Exp
      /  |  \
    Exp  *   Exp
     |      /  | \
    Num   Exp  +  Exp
     |     |       |
    Cif   Num     Num
     |     |       | 
     3    Cif     Cif
           |       |
           2       5
Leggendo le foglie da destra a sinistra troviamo 3 * 2 + 5, quindi sì, è un'espressione corretta. Inoltre, risalendo e calcolando il calcolabile troviamo Exp + Exp = 2 + 5 = 7 e, risalendo ancora, Exp * Exp = 3 * 7 = 21

La grammatica è però ambigua, e anche un altro albero è possibile (uno in cui la moltiplicazione ha la precedenza, dando come risultato 30). Ancora una volta, il problema è nella doppia ricorsione.

Modifichiamo Exp in Exp -> Prod | Prod + Exp e aggiungiamo Prod -> Num | Num * Prod

Così facendo avremo rimosso l'ambiguità restituendo un sistema che dà la precedenza alla moltiplicazione.

## Relazione tra automi e grammatiche
Come già visto, un ASF può essere tradotto sempre in una grammatica. Proviamo a formalizzare la situazione:

Dato un ASF A = (Σ, Q, S, F, δ), una grammatica equivalente a A è G = (Σ, Q, S, P) [**N.B.:** sia Σ che Q che S sono le stesse di A. Infatti, stiamo lavorando sullo stesso alfabeto con lo stesso punto di partenza. Il punto cruciale, invece, è l'utilizzare gli stati come categorie sintattiche.], dove:
* se < q, a, q' > appartiene a δ, q -> aq' appartiene a P
* inoltre, se q' appartiene a F, allora devo aggiungere ANCHE la seguente produzione q->a appartenente a P

Se faccio sistematicamente queste assunzioni per tutte le transizioni in δ ottengo una grammatica equivalente ad A.

Le produzioni ottenute dalla traduzione di un automa hanno una forma particolare: sulla destra di ogni produzione abbiamo O un simbolo dell'alfabeto O un simbolo dell'alfabeto seguito da una categoria sintattica. Le grammatiche che rispettano questa struttura sono dette ***grammatiche regolari***.

Va da sé che le grammatiche regolari siano un sottoinsieme delle grammatiche libere, ma ha senso chiedersi se, dato che un ASF può sempre essere trasformato in una grammatica regolare, se il contrario sia possibile. La risposta è sì. Banalmente invertire il processo ci restituisce un ASF (spesso ASDND, ma comunque convertibile a sua volta in un ASFD se dovesse servire). Da ciò ricaviamo che ***ASF e grammatiche regolari sono equivalenti***.

Corollario immediato è che esistono linguaggi esprimibili tramite grammatiche libere (ovvero senza restrizioni ulteriori) che non possono avere un equivalente ASF. Infatti, se ciò fosse possibile, significherebbe che avrei una grammatica regolare associata all'ASF, e che quindi le grammatiche regolari e le grammatiche libere sono equivalenti, contraddicendo il fatto che le regolari siano un sottoinsieme delle libere.

## Pumping Lemma
Dato un linguaggio libero, come capisco se è regolare? La risposta non è immediatissima, infatti dato L = {a<sup>n</sup>b<sup>m</sup>, m > n > 0} non è di immediata comprensione se questo sia regolare.

Per rispondere, utilizziamo il ***Pumping Lemma***: dato un linguaggio L, se L è regolare allora esiste n appartenente ai naturali tale che per tutte le stringhe w appartenenti a L con |w| >= n vale che esistano x, y, z appartenenti a Σ* tali che w = xyz con:
1. |xy|<=n
2. y != ε
3. xy<sup>i</sup>z appartiene a L per qualsiasi i >= 0.

Ex.: L = {a<sup>k</sup>b<sup>n</sup>, k,n > 0}

Con n = 3 abbiamo che aab appartiene a L (aab sarebbe il nostro xyz), |aab|=3>=n e che:
1. |xy| = |aa| = 2 <=n
2. y = a != ε
3. i > 0, xy<sup>i</sup>z=aa<sup>i</sup>b=a<sup>i+1</sup>b che appartiene a L.