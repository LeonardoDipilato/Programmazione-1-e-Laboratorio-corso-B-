# Lezione 22 (mercoledì 15 novembre 2017)
## Recap
* X = T(X) è un'**equazione ricorsiva** perché X è definito, appunto, *ricorsivamente* come punto fisso di T.
* T : P<sub>A</sub> -> P<sub>A</sub> è una *trasformazione di insiemi*
* Una data T può avere un qualsiasi numero di punti fissi, anche 0 o infiniti eventualmente.
* T si dice **monotona** se dati X e Y tali che X sia sottoinsieme di Y ALLORA T(X) è sottoinsieme di T(Y).
* T si dice **continua** se data una successione di insiemi X<sub>i</sub> tali che X<sub>n</sub> è sottoinsieme di X<sub>n+1</sub> ALLORA T(unione di tutti gli X<sub>i</sub>) = Unione per tutte le i di T(X<sub>i</sub>) [ovvero, detto a parole, la trasformazione applicata all'unione è uguale all'unione delle trasformazioni applicate]
* Vale, inoltre, che se T è continua ALLORA T è monotona

Per dimostrare quest'ultimo punto, assumiamo T continua. A noi serve dimostrare che dati X e Y tali che X sia contenuto in Y, T(X) sia contenuto in T(Y). Dato che X e Y formano una sequenza finita di insiemi non decrescente, per la continuità di T abbiamo che T(X unito Y) = T(X) unito T(Y). Ma, poiché X è contenuto in Y, X unito Y è uguale a Y. Da ciò abbiamo che T(Y) = T(X) unito T(Y). L'unico modo per far sì che quest'ultima sia vera è che T(X) sia contenuto in T(Y). Abbiamo dunque la monotoneità.

## Teorema di ricorsione
### Enunciato
Sia T : P<sub>A</sub> -> P<sub>A</sub> una trasformazione di insiemi ***continua***. Allora:
1. I = Unione per tutti gli i >= 0 di T<sup>i</sup>({}) è un punto fisso di T.
2. Dato J appartenente a P<sub>A</sub> tale che J = T(J), allora I è contenuto in J.

Si noti che con la notazione T<sup>i</sup> si intende la trasformazione applicata i volte a se stessa. Quindi T<sup>0</sup>({}) = {} per qualsiasi T, mentre T<sup>1</sup>({}) = T({}) e T<sup>4</sup>({}) = T(T(T(T({})))). Se volessimo definire matematicamente questo concetto, dovremmo dire che T<sup>i</sup>({}) = {} se i == 0 | T(T<sup>i-1</sup>({})) se i > 0, con i appartenente ai naturali.

### Interpretazione del teorema
Posso sempre trovare un punto fisso calcolando I = unione per i >= 0 di T<sup>i</sup>({}). In tal caso, I è il **minimo punto fisso** di T (ovvero tutti gli altri punti fissi per la stessa T contengono I).

Immaginiamo quindi di avere X = {0} unito X. L'equazione ricorsiva rabbe quindi T(X) = {0} unito X. Dato che T è continua possiamo applicare il teorema per trovare il punto fisso da usare come soluzione canonica. Calcoliamo quindi I = unione di T<sup>i</sup>({}). Per elencazione:
* T<sup>0</sup>({}) = {}
* T<sup>1</sup>({}) = {0}
* T<sup>2</sup>({}) = {0} unito {0} = {0}

### Dimostrazione
Consideriamo, in primis, un lemma utile:
> Sia T : P<sub>A</sub> -> P<sub>A</sub> continua, allora per ogni i >= 0 vale che T<sup>i</sup>({}) è contenuto in T<sup>i+1</sup>({})

Proviamo quindi a dimostrare il teorema per **induzione**. L'induzione è un metodo per dimostrare che una proprietà P vale per tutti gli elementi di un insieme infinito. Il metodo dice che se:
1. P(0) è vera
2. Se P(n) => P(n+1) è vera

Allora P(n) vale per tutti gli n.

Dimostriamo, prima di tutto, il lemma. La nostra proprietà è P(i) = [T<sup>i</sup>({}) contenuto in T<sup>i+1</sup>({})]. Applichiamo l'induzione:
1. P(0): {} è contenuto in T({})? Sì, perché l'insieme vuoto è sempre contenuto in un qualsiasi altro insieme.
2. P(n): Se T<sup>n</sup>({}) è contenuto in T<sup>n+1</sup>({}), allora è vero che T<sup>n+1</sup>({}) è contenuto in T<sup>n+2</sup>({})? Per continuità di T sappiamo che T è monotona. Essendo monotona, è vero che applicando T ad entrambi i membri di T<sup>n</sup>({}) contenuto in T<sup>n+1</sup> otteniamo ancora un'affermazione vera. Ma dato che T(T<sup>i</sup>({})) = T<sup>i+1</sup>({}) ALLORA otteniamo che T<sup>n+1</sup>({}) è contenuto in T<sup>n+2</sup>({}). Il lemma è dunque dimostrato.

