# Lezione 16 (mercoledì 25 ottobre 2017)
## Esercizio 4 del II appello 15/16
Si scriva una funzione C
`int check(int a[], int dima, int b[], int dimb)` che dati due array di interi a b e le lore dimensioni dima, dimb, restituisce il valore di verità della seguente: esiste i appartenente a [0, dima) tale che ((per qualsiasi j appartenente a [0, dimb) a[i] != b[j]) AND (#{k tale che k appartiene a [0, dima) AND a[i] == a[k]} == 1)

Partiamo dall'intuizione: detto a parole, l'affermazione significa che deve esistere un valore dell'array a tale che esista un valore di b diverso dal valore scelto di a e che non esistano altri elementi uguali a quello scelto in a. Detta ancora più striminzita, ci chiediamo se in a c'è un elemento **assolutamente** unico sia in a che in b.

Per risolvere il problema, proviamo l'approccio sistematico, risolvendo i problemi uno per volta. Ad esempio, per risolvere la prima parte del problema (controllare che non ci siano elementi di b uguali ad un dato elemento di a), scriviamo il seguente codice:
```
int diversi(int b[], ind dimb, int x) {
    int j = 0;
    int OK = 1;
    while (j < dimb && OK) {
        if (b[j] == x) OK = 0;
        j++;
    }
    return OK;
}
```

Scriviamo poi del codice che ci risolva la seconda parte (il controllare che quell'elemento sia unico nell'array):
```
int unico(int a[], int dima, int i) {
    int j = 0;
    int OK = 1;
    while (j < dima && OK) {
        if (i != j && a[i] == a[j]) OK = 0;
        j++;
    } 
    return OK;
}
```
Ora che abbiamo le due funzioni, possiamo riscrivere la richiesta come: esiste i appartenente a [0, dima) tale che diversi(b, dimb, a[i]) AND unico(a, dima, i). Per farlo, scriviamo una funzione che usi come controlli le due funzioni che abbiamo già scritto:
```
int check(int a[], int dima, int b[], dimb) {
    int i = 0;
    int trovato = 0;
    while (i < dima && !trovato) {
        if (diversi(b, dimb, a[i]) && unico(a, dima, i))
            trovato = 1;
        i++;
    }
    return trovato;
}
```

## Esercizio 2 del I compitino 16/17
Si scriva una funzione C `int contaUnico(int a[], int b[], int dima, int dimb)` che, dati due array a e b di dimensioni rispettivamente dima e dimb, restituisce il numero di elementi diversi di a che sono contenuti in b.

Dato che dovremo controllare se un elemento è parte di un array o meno, ci conviene utilizzare la funzione member già definita in precedenza:
```
int member(int b[], int dimb, int x) {
    int i = 0;
    int trovato = 0;
    while (i < dimb && !trovato) {
        if (b[i] == x) trovato = 1;
        i++;
    }
    return trovato;
}
```
Per evitare di controllare due volte lo stesso numero, quello che faremo è controllare se un numero era già stato controllato in precedenza, ignorandolo in caso lo si abbia già visto, studiandolo in caso non sia stato visto. Scriviamo quindi:
```
int cerca(int a[], int dim, int i) {
    int j = 0;
    int trovato = 0;
    while (j < i && !trovato) {
        if (a[j] == a[i]) trovato = 1;
        j++;
    }
    return trovato;
}
```

Componiamo il tutto in:
```
int contaUnico(int a[], int b[], int dima, int dimb) {
    int cont = 0;
    for (int i = 0; i < dina; i++) {
        if (!cerca(a, dima, i))
            if (member(b, dimb, a[i])) cont++;
    }
    return cont;
}
```

## Esercizio 1 del I appello 16/17
Si scriva una funzione C che, dato un array a di dimensione dim e un numero naturale n tale che 1 <= n <= dim restituisce il valore di verità di: per qualsiasi j appartenente a [n, dim), a[j] == somma su i di a[i], con i che va da [j-n, j).

Ancora una volta, intuitivamente significa che data una "distanza" n, gli elementi distanti al massimo n (ma con indice minore) da j, quando sommati, sono uguali a a[j].

Occupiamoci, quindi, di calcolare la somma di numeri in un array dato l'indice di partenza e quello di arrivo:
```
int somma(int a[], int dim, int inizio, int fine){
    int s = 0;
    for (int i = inizio; i < fine; i++) s+= a[i];
    return s;
}
```
Ora il problema si riduce a: per qualsiasi j appartenente a [n, dim), (a[j] == somma(a, dim, j-n, j)).

```
int check(int a[], int dim, int n) {
    int j = n;
    int OK = 1;
    while (j < dim && OK) {
        if (a[j] != somma(a, dim, j-n, j)) OK = 0;
        j++;
    }
    return OK;
}
```

## Esercizio 1 del II appello 16/17
Si scriva una funzione C che, dato un array a di dimensione dim, restituisce il valore di verità di: per qualsias i appartenente a [2, dim), a[i] == (somma su j di a[j], con j appartenente a [1, i - 1]) + (somma su k di a[k] per k appartenente a [0, i - 2])

Vorrei riciclare la funzione somma scritta prima, ma mi serve che la mia funzione prenda estremi inclusi, mentre `somma` escludeva la fine. Aggiriamo il problema chiamando la funzione su fine+1 che è perfettamente equivalente ad avere una funzione che sommi includendo l'estremo superiore. Otteniamo quindi:
```
int check(int a[], int dima) {
    int OK = 1;
    int i = 2;
    while (i < dim && OK) {
        if (a[i] != somma(a, dima, 0, i-1) + somma(a, dima, 1, i)) OK = 0;
        i++;
    }
    return OK;
}