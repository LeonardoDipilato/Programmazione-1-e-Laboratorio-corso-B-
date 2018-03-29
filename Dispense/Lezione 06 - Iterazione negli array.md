# Lezione 05 (venerdì 29 settembre 2017)

## Enfasi sul concetto di array come puntatore
Come già accennato, un array altro non è che un puntatore implicito. Infatti, se ho un array `a` e un puntatore `p`, `p = a` è un operazione legale in quanto sono entrambi dello stesso tipo e corrisponde ESATTAMENTE a fare `p = &a[0]`. Di contro, non è possibile fare `a = p` in quanto staremmo cercando di variare l'indirizzo del primo oggetto di un array.

## Aritmetica dei puntatori
Una volta compreso che parlare di array `a` coincide con il parlare di `&a[0]`, ci si potrebbe chiedere quale sia il significato effettivo di `a[2]`. Per spiegarlo, dobbiamo introdurre l'*aritmetica dei puntatori*.

Dato un puntatore `p`, il comando `p = p + 1` è, sorprendentemente, legale e indica l'indirizzo che avrebbe una variabile con lo stesso tipo di quella a cui punta p situata immediatamente dopo quest'ultimo.

## Problem solving su array
Ammettiamo di voler scrivere una funzione che cerca se un elemento è presente in un array, restituendo 1 se l'elemento esiste, 0 altrimenti. Per controllare l'intero array potremmo pensare di controllare tutti gli elementi uno per uno, in ordine, dal primo all'ultimo. Questo tipo di ricerca si dice **ricerca lineare incerta** (lineare perché il tempo medio di esecuzione sale linearmente con la lunghezza dell'array; incerta perché non so a priori se l'elemento esiste o meno).

Potremmo implementare tale funzione come segue:
```
int member(int a[], int dim, int elem) {
    for (int i = 0; i < dim; i++) {
        if (a[i] == elem) return 1;
    return 0;
    }
}
```
***N.B.:*** in genere, è preferibile evitare di inserire un return all'interno di un ciclo for. Questo perché, dato che il ciclo for è un ciclo iterativo con un numero noto a priori di ripetizioni, potremmo confondere il lettore del codice terminando il for prima del previsto. Più elegantemente potremmo fare:
```
int member(int a[], int dim, in elem) {
    int i = 0;
    while (i < dim && a[i] != x)
        i++;
    if (i == dim) return 0;
    else return 1;
}
```
