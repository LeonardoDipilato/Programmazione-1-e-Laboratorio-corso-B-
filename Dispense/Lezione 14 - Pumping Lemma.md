# Lezione 14 (venerdì 20 ottobre 2017)
## Pumping Lemma
Se L è un linguaggio **regolare** allora esiste n appartenente ai naturali tale che per tutte le stringhe w di L con |w| >= n vale che esiste xyz che costituiscono w come w=xyz tale che:
1. |xy| <= n
2. y != ε
3. xy<sup>i</sup>z appartiene a L per i >= 0

## Dimostrazione Pumping Lemma
* Dato che il linguaggio è regolare, esiste un automa che lo accetta.
* L'automa ha un insieme finito di stati, per poter accettare una stringa di lunghezza maggiore del numero dei suoi stati l'automa deve per forza eseguire un ciclo.
* Ma, se c'è un ciclo, allora può essere percorso un numero arbitrario di volte.
* Esistono infinite esecuzioni diverse dell'automa, in ognuna delle quali percorro un numero di volte diverso il ciclo raggiungendo sempre lo stato finale.
* Posso rappresentare come w = xyz dove x rappresenta la porzione della stringa accettata dalle transizioni, y rappresenta la porzione della stringa accettata percorrendo il ciclo mentre z rappresenta la posizione di stringa che va dal ciclo allo stato finale. Per ciò, sia xz che xyz, che xyyz, etc... devono essere accettabili.
* n lo posso scegliere come il numero degli stati dell'automa.

## Dimostrazioni con il Pumping Lemma
Rileggendo l'enunciato, è chiaro che ci sia un problema con il Pumping Lemma. Chiamano P tutte le condizioni, abbiamo che se L è regolare => P. Purtroppo, questo ci dà molte poche informazioni su L. Infatti, se l'implicazione fosse stata al contrario (se P => L è regolare) avremmo potuto utilizzare il Pumping Lemma per dimostrare che un linguaggio è regolare. Così, invece, sappiamo solo che se L è regolare allora vale P, ma se P vale non sappiamo dire nulla su L. Il lato positivo, però, è che lo si può usare per dimostrare l'irregolarità di *alcuni* L irregolari. Infatti, se abbiamo un L che non rispetta P, allora L è necessariamente irregolare (ma, repetita iuvant, L potrebbe essere irregolare pur valendo P).

Dal punto di vista logico matematico, se R => P è equivalente a se NOT P => NOT R. Quindi, se non vale P => non è regolare. Questo tipo di affermazione si dice **contropositiva**.

## Applicazione del Pumping Lemma contropositivo
Se non vale che [esiste n appartenente ai naturali tale che per tutte le stringhe w di L con |w| >= n vale che esiste xyz che costituiscono w come w=xyz tale che:
1. |xy| <= n
2. y != ε
3. xy<sup>i</sup>z appartiene a L per i >= 0]

**ALLORA** L non è regolare.

La negazione di P, coincide con: per qualsiasi n appartenente ai naturali, esiste w appartenente a L tale che ((|w|>=n) AND ((per qualsiasi xyz tali che ((w = xyz) AND (|xy| <= n) AND (y != ε) => (esiste i >= 0 tale che xy<sup>i</sup>z non appartiene a L)))))

Ad esempio, dato L = {a<sup>k</sup>b<sup>k</sup>, k > 0}, dimostriamo che NON è regolare.

* L'inizio del Pumping Lemma contropositivo dice che "per QUALSIASI n appartenente ai naturali...". Perciò, assumeremo n arbitrario.
* "Esiste w appartenente a L tale che ((|w|>=n) AND (...))" ci dice che dobbiamo trovare una stringa lunga almeno n per continuare. Prendiamo, ad esempio, w = a<sup>n</sup>b<sup>n</sup>, in maniera tale che |w|=2n>=n per qualsiasi n.
* Devo far vedere che in qualsiasi modo suddivida w=xyz, xy<sup>i</sup>z NON appartenga a L. In tal caso, ottengo che x=a<sup>h</sup> (con h >= 0, h < n), y = a<sup>t</sup> (con t > 0 STRETTAMENTE, t <= n), z = a<sup>k</sup>b<sup>n</sup> (con k=n-h-t). Ora, se vediamo i = 0, abbiamo che a<sup>h+k</sup>b<sup>n</sup> non appartiene a L. Di conseguenza abbiamo che la proprietà negata è soddisfatta e quindi il linguaggio non è regolare