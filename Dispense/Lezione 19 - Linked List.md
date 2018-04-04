# Lezione 19 (mercoledì 8 novembre 2017)
## Manipolazione Linked List
### Creazione di una Linked List
Ex. scriviamo un programma che crei una lista di 10 elementi contenenti i numeri da 1 a 10:
```
struct element {int info; struct elemento *next; }
typedef struct elemento ElementoDiListo
typedef ElementoDiLista* ListaDiElementi;

main() {
    ListaDiElementi lista = NULL;
    ListaDiElementi new = malloc(sizeof(ElementoDiLista)); // creo solo il primo elemento
    new -> info = 1; // do al primo elemento il valore che dovrebbe avere
    lista = new; // dico alla lista che il primo elemento è new
    
    for (int i = 2; i <= 10; i++) {
        new -> next = malloc(sizeof(ElementoDiLista)); // creo un nuovo elemento e lo conservo nel puntatore di new (che nella prima iterazione è il primo elemento della lista)
        new = new -> next // per ripetere il ciclo e aggiungere un nuovo elemento dobbiamo far diventare new l'ultimo elemento della lista in ogni istante
        new -> info = i; // diamo all'elemento il valore che dovrebbe avere
    }
    new -> next = NULL; // l'ultimo elemento dovrebbe avere un puntatore nullo per sapere che la lista è terminata
}
```
### Aggiunta di un elemento in testa
```
ListaDiElementi new = malloc(sizeof(ElementoDiLista)); // questo sarà l'elemento da aggiungere
new -> info = x;
new -> next = l; // dove l è il primo elemento della lista a cui vogliamo aggiungere new
l = new; // stiamo dicendo al programma che ora l'elemento in testa alla lista l è new
```
Il problema è che se volessi incapsulare questo codice in una funzione da riutilizzare poi, l = new sarebbe inutile in quanto modifica solo la "versione interna" di l, e non modifica nulla all'esterno. Dovremmo quindi scrivere la nostra funzione come segue:
```
void addT (ListaDiElementi *l, int x) {
    ListaDiElementi new = malloc(sizeof(ElementoDiLista));
    new -> info = x;
    new -> next = *l;

    *l = new;
}
```
Che andrà poi chiamata con `addT(&lista, 8)`.

Notiamo, inoltre, che `addT()` funzione anche su una lista vuota (ovvero se lista == NULL). Di conseguenza, usando solo `addT()` potremmo costruire una lista:
```
main() {
    ListaDiElementi lista = NULL;
    addT(&lista, 5);
    addT(&lista, 2);
    addT(&lista, 7); // il risultato finale sarà lista -> 7 -> 2 -> 5
}
```

### Aggiunta di un elemento in coda
```
void addC(ListaDiElementi *l, int x) {
    ListaDiElementi new = malloc(sizeof(ElementoDiLista));
    new -> info = x;
    new -> next = NULL;
    if (*l == NULL)
        *l = new;
    else {
        ListaDiElementi corr = *l;
        while (corr -> next != NULL) {
            corr = corr -> next;
        }
        corr -> next = new;
    }
}
```
Anche qui, `addC()` funziona su una lista vuota e potremmo utilizzarla per creare una lista. Purtroppo, però, la chiamata di `addC()` è molto più *costosa* (meno efficiente) di quella di `addT()` visto che deve scorrere la lista dall'inizio ogni volta. Perciò sarebbe preferibile, se non è strettamente necessario avere `addC()`, utilizzare `addT()` dove possibile.