# Lezione 04 (martedì 26 settembre 2017)

## Struttura di un programma C
Aggiornandoci a ciò che sappiamo ora, un programma C è della seguente forma:

1. Instruzioni per il preprocessore (precedute da `#`, vengono lette dal compilatore e sostituite con del codice equivalente prima di compilare il resto del programma)
2. Dichiarazioni (variabili globali, accessibili da tutti i sotto-blocchi)
3. Dichiarazione di funzioni (funzioni ausiliari che vengono chiamate all'interno del main)
4. `main() { ... }` (blocco main, una funzione che viene automaticamente chiamata all'inizio dell'esecuzione di un programma C)

Ogni funzione (incluso il main) è una sequenza di comandi, comandi che possono essere dei seguenti tipi:
* Assegnamenti (modifica dello stato, quindi è possibile sia dichiarare nuove variabili che cambiarne il valore)
* Controllo di flusso (è possibile deviare dal percorso "lineare" del programma tramite comandi iterativi o condizionali)
* Blocchi (sequenze di comandi raggruppate in uno solo)

Nei comandi è possibile inserire espressioni che, invece, leggono lo stato. (e sono gli unici elementi di un programma capaci di leggerlo).

Se all'interno di una procedura o di una funzione (indicate con p/f in questo paragrafo, per semplicità) viene chiamata un'altra p/f, la p/f corrente viene sospesa per far spazio alla p/f chiamata. Al termine dell'esecuzione della p/f chiamata viene ripresa l'esecuzione, da dove è stata lasciata, della p/f originale.

## Principio di modularità
Ogni pezzo di programma che ha un senso di per sé è bene che venga implementato come funzione o procedura. L'utilità di far ciò è di avere del codice di cui siamo certi che l'esecuzione avvenga correttamente e funzioni come previsto che venga poi utilizzato "a scatola nera" (ignorando il funzionamento ma utilizzandone solo l'interfaccia). Questo aiuta sia a trovare errori molto più speditamente in caso di malfunzionamento del proprio prorgramma, sia per rendere più leggibile il tutto.

## Puntatori
Un puntatore è una variabile il cui valore è un indirizzo di memoria. L'uso principale di un puntatore ha a che fare con il puntare all'indirizzo di un'altra variabile, per potervi accedere per indirizzo.

La dichiarazione di un puntatore è nella forma `TIPO *nomeVariabile;` (si noti l'asterisco), dove TIPO è il tipo della variabile a cui vogliamo puntare (o, meglio, come leggere i dati contenuti a quello specifico indirizzo di memoria) e nomeVariabile è il nome della variabile che andremo a dichiarare. Se, quindi, ad esempio, volessi creare un puntatore q ad un int, la dichiarazione sarebbe `int *q;`

Se invece volessi associare ad un puntatore l'indirizzo di una variabile, ciò va fatto tramite l'operatore `&`. Se x è una variabile, `&x` restituisce l'indirizzo di x. In tal caso, se volessi dichiarare un puntatore p alla variabile (intera) x l'espressione sarebbe della forma `int *p = &x;`. Si noti, però, che mentre è possibili cambiare il valore di x con comandi tipo `x = y + 1` (dove y è una variabile intera), non è possibile modificare il valore contenuto in x tramite p utilizzando la forma `p = y + 1`, in quanto `y + 1` ha tipo int, mentre p ha tipo int*.

Se volessi modificare x avendo solo il puntatore ad essa, dovrei **dereferenziare** p tramite l'operatore `*`. Infatti, `*p` restituisce esattamente la variabile x (che, essendo nel nostro caso di tipo int, si avrà che anche `*p` è di tipo int). Sarà quindi possibile modificare il valore di x tramite `*p = y + 1` oppure utilizzare il valore di x all'interno di altre espressioni tramite p come `y = *p + 4`.

## Passaggio per indirizzo
Come detto in precedenza, formalmente è possibile passare parametri ad una funzione solo per valore. Ma, se l'obiettivo fosse modificare il valore di una delle variabili passate è possibile aggirare la restrizione tramite i puntatori.

Ad esempio, dati x e y (int) contenenti dei valori, se volessimo scrivere una funzione per scambiare i loro valori potremmo utilizzare i puntatori:
```
void scambia(int *p, int *q) {
    int tmp;
    tmp = *p;
    *p = *q;
    *q = tmp;
}
```
Dato che ciò che passo alla funzione sono due int* (ovvero due indirizzi), quando vado a dichiarare nuove variabili all'interno della funzione per il passaggio per valore e le assegno i loro valori (ovvero gli indirizzi delle vere variabili esterne alla funzione), quando vado a dereferenziare la variabile interna ottengo il vero indirizzo della variabile esterna. Infatti, nonostante la funzione scambia non restituisca output tramite return, riesce a modificare lo stato. Questo si dice "effetto collaterale".

Gli effetti collaterali vengono utilizzati principalmente quando si deve restituire più di un risultato. Ad esempio, se volessi una funzione che calcola sia la somma che il prodotto di due int e volessi restituire entrambi, ciò non sarebbe possibile con un semplice return. Se, invece, passassimo come ulteriore argomento un puntatore ad una variabile che deve tenere uno dei due valore (ad esempio la somma), potremmo salvarci uno dei due e restituire l'altro tramite return.

Ex.:
```
int sommaprodotto(int x, int y, int *s) {
    // salva x + y in *s e restituisce x * y
    *s = x + y;
    return x * y;
}