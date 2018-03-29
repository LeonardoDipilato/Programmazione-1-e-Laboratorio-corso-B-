# Lezione 7 (martedì 2 ottobre 2017)

## Problem solving su array
Riprendiamo l'esempio della ricerca di un elemento in un array:
```
int member(int a[], int dim, int x) {
    int i = 0;
    int Trovato = 0;                // Variabili utilizzate in questa maniera vengono dette "flag"
    while (i < dim && !Trovato) {
        if (a[i] == x) Trovato = 1;
        i++;
    }
    return Trovato;
}
```
Questa funzione può essere tradotta, in linguaggio matematico, con `Esiste i appartenente a [0, dim) tale che a[i] == x?`.

Altri esempi:
* `Esiste i appartenente a [0, dim) tale che a[i] < 0?`
* `Esiste i appartenente a [0, dim) tale che a[i] % 2 == 0?`
* `Esiste i appartenente a [0, dim) tale che a[i] == a[i + 1]`

Si può tracciare un'analogia fra questi che ci fa capire che l'espressione per scoprire tramite ricerca lineare incerta se ALMENO un valore ha una determinata proprietà è `Esiste i appartenente a [0, dim) tale che P?` (dove P è la proprietà che l'elemento deve avere). In codice, questo si scrive come:
```
int i = 0;
int Trovato = 0;
while (i < dim && !Trovato) {
    if (GUARDIA CHE CONTROLLA CHE a[i] RISPETTI P) Trovato = 1;
    i++;
}
return Trovato;
```

Se volessimo invece controllare che TUTTI gli elementi di un array rispettino la proprietà desiderata, matematicamente avremmo che `per qualsiasi i appartenente a [0, dim), P`. Non avendo una base per questa tipologia di codice, cerchiamo di ricondurla a quella di prima che invece abbiamo analizzato. Notiamo, infatti, che l'affermazione che deve controllare che tutti gli elementi rispettino la proprietà è equivalente a dire che `esiste i appartenente a [0, dim) tale che ~P` sia un'affermazione falsa. Da questo ricaviamo il seguente codice:
```
int i = 0;
int Trovato = 0;
while (i < dim && !Trovato) {
    if (GUARDIA CHE CONTROLLA CHE a[i] RISPETTI ~P) Trovato = 1;
    i++;
}
return !Trovato;
```

## Problemi di conta
Ammettiamo di voler contare quanti elementi di un array rispettino una determinata proprietà P. In linguaggio matematico, ciò che vogliamo è `Cardinalità di {i tale che (i appartiene a [0, dim) AND P}`

In codice:
```
int contaSeP(int a[], int dim) {
    int conta = 0;
    for (int i = 0; i < dim; i++) {
        if (GUARDIA CHE CONTROLLA CHE a[i] RISPETTI P) conta++;
    }
    return conta;
}
```

## Problemi di aggregazione
Immaginiamo di voler sommare tutti i gli elementi di un array che rispettano una data proprietà P. In linguaggio matematico, `Sommatoria b[i] per i che appartiene a [0, dim(B)), dove B = {a[i], i appartenente a [0, dim) tali che P}`.

In codice:
```
int sommaElementi(int a[], int dim) {
    int somma = 0;  // detto accomulatore
    for (int i = 0 i < dim; i++) {
        if (GUARDIA CHE CONTROLLA CHE a[i] RISPETTI P) somma += a[i];
    }
    return somma;
}
```

## Ricerca del minimo elemento
L'obiettivo è scrivere una funzione che restituisca il più piccolo elemento di un array. In linguaggio matematico, vogliamo `x tale che esista i per la quale a[i] == x e tale che per qualsiasi j appartenente a [0, dim), x <= a[j]`.

Come prima, possiamo costruire questa funzione assemblandone di più piccole che siano, però, note. Ad esempio, la parte del qualsiasi può essere scritta con la negazione (come fatto in precedenza). In codice:
```
int minoreDiTutti (int a[], int dim, int x) {
    int i = 0;
    int OK = 1;
    while (i < dim && OK) {
        if (x > a[i]) OK = 0;
        i ++;
    }
    return OK;
}
```

Ora si può iterare attraverso l'array per risolvere il problema chiamando la funzione minoreDiTutti:
```
int minimo(int a[], int dim) {
    int i = 0;
    int Trovato = 0;
    int min;
    while (i < dim && !Trovato) {
        if (minoreDiTutti(a, dim, a[i])) {
            min = a[i];
            Trovato = 1;
        }
    }
    return min;
}
```

La soluzione ottenuta per ricombinazione è **sicuramente** corretta, ma di contro non è detto sia ottimale. Infatti, stiamo iterando attraverso l'array molte più volte del necessario rispetto a se si, per esempio, salvasse il valore più piccolo trovato in ogni dato momento.
```
int minimo(int a[], int dim) {
    int min = a[0];
    for (int i = 1; i < dim; i++) {
        if (a[i] < min) min = a[i];
    }
    return min;
}
```