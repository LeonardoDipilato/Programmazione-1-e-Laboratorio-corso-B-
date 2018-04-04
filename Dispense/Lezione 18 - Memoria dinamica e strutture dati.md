# Lezione 18 (martedì 7 novembre 2017)
## Memoria dinamica
Ciò che abbiamo visto fino ad ora vedeva la memoria nello stato di un programma come una pila di frame (**stack**). I frame in memoria corrispondono ai blocchi contenuti nel programma, così da poter separare variabili locali da variabili globali. Far questo, però, comporta delle limitazioni: bisogna conoscere a priori quanta memoria allocare ad un determinato frame perché, se eccedessimo, potremmo sovrascrivere dati dal frame successivo. Più notevolmente, questo problema si realizza quando vogliamo prendere n input dall'utente e salvarli in un array. Dato che non sappiamo n a priori, l'unica cosa che possiamo fare è immaginare il numero più grande di input che l'utente possa mettere e creare un array di quella dimensione. Ovviamente, fare ciò ha delle pecche piuttosto ovvie: in primis, l'utente potrebbe aver bisogno di inserire n+1 input, superando così le nostre previsioni, e in secondo luogo, se l'utente inserisce solo, ad esempio, n/2 input, abbiamo assegnato molta memoria che non verrà mai utilizzata, il che è decisamente uno spreco.

Introduciamo quindi la **memoria dinamica** (**heap**).

## Heap
L'heap è una parte diversa della memoria in cui le variabili non vengono allocata dai blocchi ma tramite delle operazioni fatte dal programma. L'allocazione viene fatta tramite la funzione `malloc()` (**m**emory **alloc**ation) della libreria `<stdlib.h>`. Questa funzione crea una variabile nell'heap e restituisce un puntatore alla nuova variabile.

`malloc()` prende come argomento la dimensione, in byte, da allocare alla variabile. Questa dimensione varia in base al tipo di variabile che ci serve e in genere la si ottiene tramite la funzione `sizeof()` (e.g. `sizeof(int)`).

Ex. allocazione di un int sull'heap:
```
int x = 10;
int *p;
p = malloc(sizeof(int));
*p = x + 1; // l'int sull'heap ora vale 11
x = *p + 2  // x ora vale 13
```

Ex. influenza dei blocchi sulla variabile nell'heap:
```
int x = 10;
int *p;
{
    int y = 12;
    p = malloc(sizeof(int));
    *p = y + 1; // *p ora vale 13
}
x = *p // va bene, perché la variabile sullo stack, p, è stata creata in questo stesso blocco e quindi esiste, mentre l'allocazione nell'heap è dinamica e non viene sovrascritta dalla chiusura del suo blocco
```

Si noti, quindi, l'importanza di avere un puntatore nello stesso *scope* in cui stiamo lavorando (perché dato che questo andrebbe creato nello stack verrebbe sovrascritto se venisse generato assieme alla variabile sull'heap), mentre l'allocazione dinamica può avvenire dove si vuole.

***Problema:*** che succede se non creo il puntatore o se lo modifico?
```
int x = 0;
int *p = malloc(sizeof(int)); // p ora punta a questa variabile sull'heap
*p = 12;
p = malloc(sizeof(int)); // p viene "disassociata" dalla precedente e punta ora a quest'altra variabile
```
Così facendo abbiamo perso, irrimediabilmente, l'informazione su dove si trovi la prima variabile che non sarà più accessibile. Si parla, quindi, di **garbage**, ovvero spazzatura, dei rimasugli di un programma eseguito in precedenza che occupano memoria senza alcuna valida ragione.

Per evitare che il garbage si accumuli, è bene liberare l'heap dopo aver finito di utilizzare una variabile (ovvero quando siamo certi che non useremo più quell'informazione). Per farlo, usiamo la funzione `free()` che prende come argomento un puntatore alla variabile (dinamica) da liberare. Far ciò distrugge *p (ed è bene, quindi, non dereferenziare più p), ma conserva p che può quindi essere riassegnato ad un altra variabile, se necessario.

Torniamo quindi al problema originale: come allocare array di dimensione non nota a priori?
```
int dim;
scanf("%d", &dim);
int *a = malloc(dim * sizeof(int)); // se vogliamo un array con dim elementi tutti int, dobbiamo allocare dim volte lo spazio per un elemento int
for (int i = 0; i < dim; i++)
    a[i] = i;
```

## Liste
Una **lista collegata** (**linked list**) è una sequenza di elementi dello stesso tipo che può avere una dimensione variabile. Una lista è composta da una serie di **nodi**, ovvero delle strutture dati (vedremo a breve cosa sono) che hanno sia il valore da conservare sia il puntatore al nodo successivo.

Il problema con le liste è, quindi, l'assenza del cosiddetto *random access* (o *accesso diretto*). Ovvero, per un array mi basta scrivere a[i] ed ottengo il valore all'indice i immediatamente. Di contro, data la natura delle liste, l'unico modo che ho di trovare l'elemento all'indice i è di partire dal primo elemento, e seguire il puntatore, poi seguire il secondo puntatore, poi il terzo e così via.

Il vantaggio, però, è che le liste permettono l'aggiunta di nuovi elementi in un qualsiasi punto della lista o la rimozione di un qualsiasi elemento semplicemente modificando i collegamenti fra i vari nodi.

## Strutture in C
Se volessi legare fra di loro un numero finito di variabili, in maniera da accedere a loro come unico pacchetto, ciò che devo fare è costruire una struct. Una struct altro non è che una "scatola" che contiene una o più variabili.

Per definire una struct in C dobbiamo utilizzare la seguente sintassi:
```
struct elemento {
    int info;
    struct elemento *next;
}
```
Così facendo abbiamo costruito una struttura che contenga un valore int ed un "link" ad un'altra struttura dello stesso tipo. Possiamo, inoltre, rinominare la struttura tramite il costrutto **typedef**:
```
struct elemento {int info; struct elemento *next; }
typedef struct elemento ElementoDiLista // quello che ho fatto qui è creare un tipo, simile a int o char, di nome ElementoDiLista le cui variabili saranno struct elemento
typedef ElementoDiLista* ListaDiElementi; // definisco un nuovo tipo ListaDiElementi le cui variabili saranno puntatori a ElementoDiLista
```

### Applicazione
```
main() {
    ElementoDiLista elem;
    elem.info = 10;
    elem.next = NULL; // NULL è una costante che rappresenta un puntatore nullo
}
```
Grazie al punto (.) posso accedere ai componenti delle strutture.

Fare ciò, però, non ha risolto il nostro problema delle liste infinite: infatti non ho modo di creare n nodi (dove n è l'input dell'utente) dato che se le creassi all'interno di un ciclo andrebbero perse una volta terminata ogni singola iterazione. Vediamo, quindi, come effettuare questo lavoro sull'heap:
```
main() {
    ListaDiElement lista;
    lista = malloc(sizeof(ElementoDiLista));
    (*lista).info = 10; // in alternativa avrei potuto scrivere lista -> info = 10; che è esattamente lo stesso
    lista->next = NULL; // come prima, avrei potuto scrivere (*lista).info = NULL;
}