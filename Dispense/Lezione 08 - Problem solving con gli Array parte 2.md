# Lezione 08 (mercoledì 4 ottobre 2017)

## Verificare che tutti gli elementi di un array siano diversi tra loro
Dato un array, voglio controllare che non ci siano due elementi uguali. Formalizzando, abbiamo che `per qualsiasi i,j appartenenti a [0, dim), (a[i] != a[j] OR i == j)`. Se fissassimo i sapremmo risolvere il problema di controllare se P viene rispettata da ogni elemento di a. Una volta definita la funzione che svolge questo lavoro, possiamo quindi iterare per tutte le i per ottenere la soluzione.

Ancora una volta, l'approccio modulare "ovvio" ci dà una soluzione molto facile e veloce da ottenere, ma molto poco ottimale (in quanto tutte le coppie vengono valutate due volte). Per risolvere il problema, possiamo ridefinire la definizione formale in `per qualsiasi i appartenente a [0, dim-1) e per qualsiasi j appartenente a [i+1, dim), a[i] != a[j]`.

In tal caso, possiamo notare che, chiamando a[i] = x, la funzione è molto simile alla member che parte da una posizione avanzata di 1. In tal caso:
```
int memberi(int a[], int dim, int i) {
    int j = i + 1;
    int OK = 1;
    while (j < dim && OK) {
        if (a[i] == a[j]) OK = 0;
        j++;
    }
    return OK;
}
```

Questa funzione può poi essere chiamata iterando per i risolvendo il problema.

### Approccio non modulare
L'approccio non modulare è sconsigliato per programmi complessi, in quanto la risoluzione in toto di un grande problema può risultare in codice confusionario, difficile da mantenere e che può nascondere una serie di problemi insiti in sé molto difficili da rimuovere. Per problemi più semplici, invece, la soluzione potrebbe essere immediata. Ad esempio, il nostro problema può essere affrontato tramite l'utilizzo di ***cicli annidati*** (due cicli uno dentro l'altro).
```
int tuttiDiversi(int a[], int dim) {
    int i = 0;
    int OK = 1;
    while (i < dim && OK) {
        int j = i + 1;
        while (j < dim && OK) {
            if (a[i] == a[j])
                OK = 0;
            j++;
        }
        i++;
    }
    return OK;
}
```

## Problemi con 2 array
Ex.: dati due array a e b di dimensioni dima e dimb verificare se TUTTI gli elementi del primo sono anche elementi del secondo.

In "matematichese", `per qualsiasi i appartenente a [0, dima), esiste j appartenente a [0, dimb) tale che a[i] == b[j]`. (Risolvere per esercizio, es.#0)

Ex.#2: dati due array a e b di dimensione dima e dimb, verificare se esista elemento in a che corrisponde alla somma degli elementi di b. In matematichese, `esiste i appartenente a [0, dima) tale che a[i] == s, dove s è la somma degli elementi di b`.

Questo è un problema di facile risoluzione utilizzando la modularità dei problemi: possiamo ad esempio utilizzare la funzione già scritta per sommare tutti gli elementi di b per poi iterare su a per controllare se qualche elemento è uguale alla somma.


### Esercizi:
1. Scrivere una funzione che dato un array di dimensione dim verifica la seguente proprietà: `per qualsiasi i appartenete a [0 dim-1), a[i] < a[i+1]`
2. " " " " " " " ": `esiste i appartenente a [0, dim) tale che a[i] == s, dove s è la somma degli elementi di a con indice minore di i`
3. " " " " " " " ": `la somma degli elementi maggiori di zero di a è uguale in modulo alla somma degli elementi negativi di a`
4. " " " " " calcolare: `cardinalità di {i tale che ((i appartiene a [1, dim-1)) AND (a[i] == s, dove s è la somma degli elementi di a con indice minore di i) AND (a[i] == s', dove s' è la somma degli elementi di a con indice maggiore di i)}
5. Dati due array a e b di dimensioni dima e dimb verificare la seguente proprietà: `per qualsiasi i appartenente a [0, dima), esiste j appartenente a [0, dimb) tale che esista k appartenente a [0, dimb) tale che ((a[i] == b[j]) AND (a[i] == b[k]))`

## Risoluzione esercizi
### Esercizio 0
```
int checkIfInb(int A, int b[], int dimb) {
    int i = 0;
    int flag = 0;
    while (i < dimb && !flag) {
        if (b[i] == A) flag = 1;
        i++;
    }
    return flag;
}

int checkIfaInb(int a[], int b[], int dima, int dimb) {
    int i = 0;
    int flag = 1;
    while (i < dima && flag) {
        flag = checkIfInb(a[i], b[], dimb);
        i++;
    }
    return flag;
}
```

### Esercizio 1
```
int checkIfIncreasing(int a[], int dim) {
    int i = 0;
    flag = 1;
    while (i < dim - 1 && flag) {
        flag = a[i] < a[i + 1];
        i++;
    }
    return flag;
}
```

### Esercizio 2
```
int checkIfSumLower(int a[], int dim) {
    int i = 0;
    flag = 0;
    while (i < dim && !flag) {
        int somma = 0;
        int j = 0;
        while (somma < a[i] && j < i) {
            somma += a[j];
        }
        flag = a[i] == somma;
    }
    return flag;
}
```

### Esercizio 3
```
int negEqualsPlus(int a[], int dim) {
    int sumNeg = 0;
    int sumPos = 0;
    for (int i = 0; i < dim; i++) {
        if (a[i] < 0)
            sumNeg += -a[i]
        else
            sumPos += a[i]
    }
    return (sumNeg == sumPos);
}
```

### Esercizio 4
```
int sumAll(int a[], int dim) {
    int somma = 0;
    for (int i = 0; i < dim; i++) {
        somma += a[i];
    }
    return somma;
}

int checkIfSumBoth(int a[], int dim) {
    int i = 0;
    flag = 0;
    while (i < dim && !flag) {
        int somma = 0;
        int j = 0;
        while (somma < a[i] && j < i) {
            somma += a[j];
        }
        flag = (a[i] == somma && a[i] = sumAll(&a[i], dim-i));
    }
    return flag;
}
```

### Esercizio 5
int checkIfDouble(int a[], int A, int dim) {
    int i = 0;
    int flag = -1;
    while (i < dim && flag <= 0) {
        flag += (a[i] == A);
    }
    return flag;
}

int member(int a[], int dim, int A) {
    int i = 0;
    int flag = 0;
    while (i < dim && !flag) {
        flag = (A == a[i]);
        i++;
    }
    return flag;
}

int checkIfDoubleCorrespondence(int a[], int b[], int dima, int dimb) {
    int i = 0;
    int flag = 1;
    while (i < dima && flag) {
        flag = (member(b, dimb, a[i]) &&
                checkIfDouble(b, a[i], dimb)) 
    }
    return flag;
}