# Lezione 23 (venerdì 17 novembre 2017)
## Recap
### Teorema di Ricorsione
Se T : P<sub>A</sub> -> P<sub>A</sub> è continua allora:
1. I = Unione per i >= 0 di T<sup>i</sup>({}) è un punto fisso di T.
2. Qualsiasi punto fisso J di T contiene I (ovvero I è il minimo punto fisso).

### Lemma
Se T : P<sub>A</sub> -> P<sub>A</sub> è continua allora per qualsiasi i >= 0 vale che T<sup>i</sup>({}) è contenuto in T<sup>i+1</sup>. (Se T continua => T monotona)

### Principio di induzione
Data una proprietà P, per dimostrare che vale per tutti i naturali è sufficiente dimostrare che:
1. P(0) sia vera
2. Che l'affermazione (P(n) => P(n+1)) sia vera

## Dimostrazione del teorema di ricorsione
Partiamo dimostrando che I = Unione per i >= 0 di T<sup>i</sup>({}) è un punto fisso di T.

I punto fisso significa che I = T(I). Allora, per definizione di I, abbiamo che T(I) = T(unione dei T<sup>i</sup>({})). Per continuità abbiamo che quest'ultima è uguale all'unione per i di T(T<sup>i</sup>({})) che è uguale, per definizione di T<sup>i</sup> all'unione per i di T<sup>i+1</sup>({}) che è uguale all'unione per i **>= 1** di T<sup>i</sup>({}). Ma poiché {} unito A = A per qualsiasi A e che T<sup>0</sup>({}) = {} abbiamo che l'unione per i >= 1 di T<sup>i</sup>({}) = unione per i >= 0 di T<sup>i</sup>({}) che è, per definizione, uguale a I. Di conseguenza, T(I) = I. Ricapitolando:

T(I) = T(unione dei T<sup>i</sup>({})) = unione per i di T(T<sup>i</sup>({})) = unione per i di T<sup>i+1</sup> = unione per i >= 1 di T<sup>i</sup> = unione per i >= 0 di T<sup>i</sup> = I. Di conseguenza, T(I) = I.

Dimostriamo ora che I è il minimo dei punti fissi.

Dobbiamo dimostrare che dato J = T(J), I sia contenuto in J. Per far ciò, ci basta vedere che, dato che I = unione T<sup>i</sup>({}), se per qualsiasi i T<sup>i</sup>({}) è contenuto in J ALLORA I è contenuto in J. Per induzione, infatti:
1. T<sup>0</sup>({}) è contenuto in J perché l'insieme vuoto è sempre contenuto in qualsiasi insieme.
2. Dato che T è continua e monotona e dato che (per passo induttivo) T<sup>n</sup>({}) è contenuto in J allora, applicando T ad entrambi i membri, si ottiene che T<sup>n+1</sup>({}) è contenuto in T(J). Ma, dato che J è un punto fisso, T(J) = J. Di conseguenza, T<sup>n+1</sup> è contenuto in J.

## Applicazione del teorema
X = {0} unito {n tali che n appartenga ai naturali e n-2 appartenga ad X}. Abbiamo, quindi, T(X) = {0} unito {n tali che n appartenga ai naturali e n-2 appartenga ad X}.

Vorremmo utilizzare il teorema, ma per farlo dobbiamo prima dimostrare la continuità di T. Data una successione X<sub>i</sub> non decrescente, dobbiamo avere che l'unione per i di T(X<sub>i</sub>) = T(unione per i di X<sub>i</sub>). Partendo dalla definizione di T abbiamo che l'unione per i di T(X<sub>i</sub>) = unione({0} unito {n tali che n appartenga ai naturali e n-2 appartenga ad X<sub>i</sub>}) che è uguale a {0} unito l'unione di {n tale che n appartenga ai naturali e n-2 appartenga a X<sub>i</sub>} che è uguale a {0} unito {n tale che n appartenga ai naturali e n-2 appartenga all'unione di X<sub>i</sub>} che, per definizione di T, è uguale a T(unione di X<sub>i</sub>). Da ciò abbiamo che T è continua.

Visto che T è continua, possiamo calcolare I:
* i = 0: T<sup>0</sup>({}) = {}
* i = 1: T<sup>1</sup>({}) = {0} unito {} = {0}
* i = 2: T<sup>2</sup>({}) = T(T({})) = T({0}) = {0} unito {n tale che n appartenga ai naturali e n-2 appartenga a {0}} = {0} unito {2}
* i = 3: T<sup>3</sup>({}) = T({0, 2}) = {0, 2, 4}

Di conseguenza, I = P, dove P è l'insieme di tutti i numeri pari (incluso 0)

## Funzioni ricorsive
Abbiamo visto che il teorema ci consente di dare definizioni ricorsive di insiemi, mentre quello che a noi interessa sono le **funzioni ricorsive**. Vedendo, però, una funzione come l'insieme {<x, f(x)> tali che x appartenga al dominio di f} otteniamo un insieme su cui lavorare. Questo insieme si chiama ***grafico*** della funzione *f*

## Grafico di una funzione ricorsiva
Prendiamo la funzione fattoriale come definita in precedenza (ricorsivamente). Ciò che otteniamo è un grafico F = {<0, 1>} *[caso base]* unito a {<n, n\*k> tale che n appartenga ai naturali, n > 0 e <n-1, k> appartenga ad F}. Applicando il teorema su questo insieme (sappiamo che sia continua perché sì, ma andrebbe dimostrato), abbiamo che:

T(X) = {<0, 1>}  unito a {<n, n\*k> tale che n appartenga ai naturali, n > 0 e <n-1, k> appartenga ad X}.
* i = 1: T({}) = {<0, 1>} unito {roba roba roba tale che <n-1, k> appartenga a {}} = {<0, 1>} unito {} = {<0, 1>}
* i = 2: T({<0, 1>}) = {<0, 1>} unito {<1, 1*1> tali che ... <n-1, k> appartenga a {<0, 1>}} = {<0, 1>} unito {<1, 1>}.
* ...

Possiamo quindi notare che questa funzione termina per qualsiasi i, il che implica che sia ben progettata (il che non è banale)
