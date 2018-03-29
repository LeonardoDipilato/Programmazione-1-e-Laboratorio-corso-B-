# Lezione 05 (mercoledì 27 settembre 2017)

## Il problema degli n-input
Immaginiamo di dover scrivere del codice che sommi 5 int arbitrari forniti dall'utente (che devono essere salvati e tenuti in memoria). Ora come ora, potremmo pensare di scrivere qualcosa tipo:
```
int x;
int y;
int z;
int w;
int k;
int TOT;
scanf("%d", &x); //scanf legge un input dall'utente e lo salva in x
... // ripetiamo per le altre quattro variabili
TOT = x + y + z + w + k;
```
Gestire così tante variabili diverse per un processo così semplice non è una buona condotta. Se invece che 5 int volessimo leggere n int, dove anche n è arbitrario, questo tipo di codice potrebbe comportare problemi.

## Array
Gli array sono una struttura dati che permette conservare un numero arbitrario di valori tutti dello stesso tipo, accessibili tramite il loro indice identificativo (in quanto ordinati, in ordine crescente, partendo da 0). Ad esempio, dichiarando un array di nome a che contiene, in sequenza, i valori 1, 3, 11, 2, 42, 1, per accedere al primo valore di a dovrei chiamare a[0] (il che restituisce 1). a[3] sarebbe, in questo caso specifico 2 e così via. (Quando parliamo di "accedere" si intende che a[n] è trattabile come una vera e propria variabile, e possiamo quindi sia ottenerne il valore che modificarlo tramite assegnazione tipo `int a[2] = 2`.)

Definiamo, inoltre, ***dimensione di un array*** il numero di elementi che esso contiene.

Per dichiarare un array a (ad esempio di interi di lunghezza 10), il comando dev'essere `int a[10]`. ***N.B.:*** se provassi ad accedere a a[10] ciò che ottengo NON è un valore dell'array in quanto un array di dimensione 10 arriva al massimo al valore 9. Se provassi ad accedere e modificare un valore oltre la lunghezza dell'array, il risultato sarebbe lo sfociare in indirizzi esterni all'array, resi sovrascrivibili, modificando possibilmente valori di altre variabili.

Inoltre, quando si dichiara un array è necessario utilizzare come dimensione un valore costante che NON può essere una variabile.

Quando si dichiara un array, spesso è bene inizializzarlo completamente vuoto (ad esempio, 0 per gli int). Per far ciò, il metodo consigliato è un loop:
```
int a[5]
for (int i = 0; i < 5; i++) {
    a[i] = 0;
}
```

Ex. azzerare tutti i valori negativi all'interno di un array a di lunghezza n:
```
for (int i = 0; i < 5; i++)
    if (a[i] < 0)
        a[i] = 0;
```

Ex.#2 somma di n elementi contenuti in un array a:
```
int sumN(int[] a, int n) {
   int sum = 0;
   for (int i = 0; i < n; i++) {
       sum += a[i];
   } 
   return sum;
}
```

***ATTENZIONE:*** quando passo un array ad una funzione non passo TUTTI gli elementi dell'array per valore (anche perché, come già detto, il computer non sa quanto sia lungo l'array effettivamente senza passargli anche la variabile che contiene la lunghezza), ma gli passo il primo oggetto per indirizzo.
