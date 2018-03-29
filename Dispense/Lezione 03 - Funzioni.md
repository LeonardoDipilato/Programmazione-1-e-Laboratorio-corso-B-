# Lezione 03 (venerdì 22 settembre 2017)
## Blocchi annidati
Si può inserire un blocco `{}` all'interno di un altro blocco. In tal caso si dice che il blocco più "interno" è **annidato**.

Quando ciò succede, le variabili dichiarate all'interno del blocco più interno non sono accessibili a quello esterno, mentre il viceversa è possibile

Ex.:
```
int x = 1;
{
    int y = 4;
    y = y + x;      // OK
    {
        int z = x;  // OK
        z = z * y;  // OK
    }
    y = z;          // ERRORE!
}
```

## Pila (Stack)
Una pila è una struttura ordinata di valori dotata di due operazioni:
* `push` che inserisce un elemento in cima alla pila
* `pop` che elimina l'elemento in cima alla pila

Si può pensare ad una pila come l'accatastamento di fogli uno sopra l'altro: quando si accumulano diversi fogli che vanno letti e ricontrollati, quelli ottenuti più di recenti vengono poggiati in cima e quando, finalmente, arriva il momento di leggere i documenti, il primo che si prende in mano è quello in cima. Perciò, le stack hanno una forma ***LIFO*** (Last In, First Out: l'ultimo arrivato è il primo ad andarsene).

***N.B.:*** si possono avere pile vuote (tipicamente indicate con la lettera ***Ω***).

Un comando push è della forma `push(pila, valore)`. Data una pila del tipo (il valore più a sinistra è quello in fondo alla pila, quello più a destra è in cima) `p = [3][4]`, e un valore `v = 7`, dopo aver eseguito il comando `push(p, v)`, p sarà uguale a `[3][4][7]`.

Un comando pop è della forma `pop(pila)`. Data una pila del tipo `p = [3][2]`, dopo il comando `pop(p)`, p diventerà uguale a `[3]`.

Possiamo quindi introdurre una nuova definizione di *stato*: uno stato è costituito da una pila di frame di ambiente (ambiente) e da un frame di memoria (memoria). Esiste un frame vuoto che si indica con **___**. [Un frame è un "dizionario" che collega il nome di una variabile al suo valore.]

Il frame vuoto viene aggiunto alla pila di ambiente ogni volta che si entra in un blocco (ovvero, quando si incontra una parentesi graffa aperta).

Quando si dichiara una variabile, avviene un'associazione fra l'elemento in cima alla pila di ambiente e quello in cima alla pila di memoria.

Quando si esce da un blocco viene eliminato il frame di memoria e di ambiente in cima, rispettivamente, all'ambiente e alla memoria.

## Funzioni
Una funzione è, similmente alla matematica, una serie di operazioni effettuate su degli input che restituisce un output (eventualmente nullo).

Se volessi, ad esempio, scrivere una funzione che calcoli il MCD di qualsiasi coppia di x e y (invece che, come fatto in precedenza, di due numeri specifici), si può generalizzare la procedura per definire una funzione (magari di nome MCD) che prenda due input qualsiasi (interi).

Ex.:
```
void MCD(int p, int s) {
    while (p != s)
        if (p > s) p = p - s;
        else s = s - p
}
```
***N.B.:*** Se dovessi chiamare `MCD(x, y)`, con `x = 32` e `y = 20` questa funzione non restituisce il valore del MCD (in quanto è di tipo **void** ma nemmeno modifica x e y. Ciò che facciamo chiamando `MCD(x, y)` è, essenzialmente, prendere i valori di x e y (32 e 20, rispettivamente) e dichiarare, inizializzando, p ed s con quei valori. Una volta copiati questi valori, la funzione non ha più nulla a che fare con x e y, e di conseguenza essi non vengono modificati in alcuna misura. Al contempo, dato che p ed s smettono di esistere una volta usciti dal blocco, essi non sono più reperibili. Dato che la funzione non restituisce alcun risultato, il MCD è perso e sono stati effettuati calcoli a vuoto.

## Passaggio dei parametri
Ciò di cui abbiamo appena parlato si chiama ***passaggio per valore*** dei parametri (ovvero, l'atto di inizializzare le variabili generiche all'interno della funzione con i valori delle variabili passategli quando viene chiamata). Formalmente parlando, il passaggio per valore è l'unica modalità di passaggio dei parametri ammessa in C. 

In tal caso, per far sì che una funzione sia, a tutti gli effetti, "utile", dovrà ammettere l'esistenza di un output. Per far ciò, la funzione deve essere definita del tipo di valore che deve restituire e, al contempo, deve avere nel suo corpo la parola chiave `return`, che resituitsce il valore alla sua destra.

Ex. MCD funzionante:
```
int MCD(int p, int s) {
    while (p != s)
        if (p > s) p = p - s;
        else s = s - p
    return p;
}
```
Ex.#2 fattoriale funzionante:
```
int fact(int n) {
    int f = 1;
    while (n > 1) {
        f = f * n;
        n = n - 1;
    }
    return f;
}
```
