# Lezione 20 (venerdì 10 novembre 2017)
## Operazioni sulle liste
Abbiamo visto nella scorsa lezione come inserire in testa e in coda ad una lista e abbiamo anche imparato che se la procedura deve modificare il primo elemento allora è necessario che si passi la lista per indirizzo, mentre se dobbiamo scorrere la lista dobbiamo utilizzare un puntatore temporaneo.

### Lunghezza di una lista
```
int length(ListaDiElementi l) {
    int cont = 0;
    while (l != null) {
        cont++;
        l = l-> next;
    }
    return cont;
}
```

### Controllare se una lista è (debolmente) crescente
```
int ord(ListaDiElementi l) {
    int ordinata = 1;
    while (l != NULL && l -> next != NULL && ordinata) {
        if (l -> info > l -> next -> info) {
            ordinata = 0;
        }
        else {
            l = l -> next;
        }
    }
    return ordinata;
}
```

### Inserimento nel mezzo di una lista
Cerca un elemento che vale x e inserisce v prima di lui.
```
void (ListaDiElementi *l, int x, int v) {
    ListaDiElementi prec, corr;
    prec = NULL;
    corr = *l;
    int trovato = 0;
    while (corr != NULL && !trovato) {
        if (corr -> info == x) {
            trovato = 1;
        }
        else {
            prec = corr;
            corr = corr -> next;
        }
    }
    if (trovato) {
        ListaDiElementi new = malloc(sizeof(ElementoDiLista));
        new -> info = v;
        new -> next = corr;
        if (prec != NULL) {
            prec -> next = new;
        }
        else {
            *l = new;
        }
    }
}
```

### Eliminazione dell'elemento in testa alla lista
```
void cancT(ListaDiElementi *l) {
    if (*l != NULL) {
        ListaDiElementi old = *l;
        *l = *l -> next;
        free(old);
    }
}
```

### Eliminazione dell'elemento in coda alla lista
```
void cancC(ListaDiElementi *l) {
    if (*l != NULL) {
        if ((*l)->next == NULL) {
            free(*l);
            *l = NULL;
        }
    }
    else {
        ListaDiElementi prec = *l;
        ListaDiElementi corr = (*l) -> next;
        while (corr -> next != NULL) {
            prec = corr;
            corr = corr -> next;
        }
        free(corr);
        prec -> next = NULL;
    }
}
```

### Spostamento di elementi
Scriviamo una funzione che scambi i primi due elementi di una lista
```
void scambiaprimiedue(ListaDiElementi *l) {
    if (*l != NULL && (*l) -> next != NULL) {
        ListaDiElementi tmp = *l;
        *l = (*l) -> next;
        tmp -> next = (*l) -> next;
        (*l) -> next = tmp;
    }
}
```