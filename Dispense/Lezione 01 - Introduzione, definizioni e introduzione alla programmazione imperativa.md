# Lezione 1 (19 settembre 2017)

### Definizione programma
> "Descrizione eseguibile da un *calcolatore* di un **metodo** per il calcolo di un *risultato* a partire da dati *iniziali*"

Il **metodo** sarebbe un algoritmo, mentre chiamiamo rispettivamente output e input il risultato e i dati iniziali.

### Paradigmi di programmazione
Esistono diverse tipologie di paradigmi (modalità, ideologie) di programmazione. Fra queste, ne spiccano tre in particolare:
* Programmazione imperativa
    * Per il paradigma della programmazione imperativa, un programma è una serie di comandi (ordini, da cui imperativo).
    * La programmazione imperativa verrà trattata in questo corso tramite il linguaggio **C**.
* Programmazione funzionale
    * Per il paradigma della programmazione funzionale, un programma è semplicemente l'esecuzione di funzioni ben definite.
    * La programmazione funzionale verrà trattata in questo corso tramite il linguaggio **Caml**.
* Programmazione a oggetti
    * Per la programmazione a oggetti, un programma è l'insieme di elementi dotati di varie funzionalità.
    * Esempi di linguaggi per la programmazione a oggetti sono Java, C++ e C#, che però non verranno trattati in questo corso.

### Argomenti del corso
* Programmazione imperativa in C
* Sintassi dei linguaggi di programmazione
* Teoria della ricorsione (programmazione funzionale)
* Programmazione funzionale in Caml

## Programmazione imperativa
Si parte da uno stato iniziale (definito da un insieme di variabili) detto *input* che viene man mano modificato. Lo stato che si ottiene quando il programma termina di operare è detto *output*

Una **variabile** è un nome associato ad un valore, in maniera simile a come associamo un valore alle incognite nella risoluzione di un problema matematica. Di contro, però, il valore di una variabile può essere modificato (ovvero, è tempo-variante), a differenza delle incognite.

Ex:
* x = 10, y = 5, z = 2, somma = 0 [INPUT]

* somma = y + z -> somma = 7 [Comando #1]

* somma = somma + x -> somma = 17 [Comando #2]

* Termine programma [OUTPUT: x = 10, y = 5, z = 2, somma = 17]

### Comando di assegnamento
Il comando di base della programmazione imperativa è l'**assegnamento**. L'assegnamento consente di modificare il valore di una variabile. Un assegnamento è della forma `Variabile = Espressione`. Quando effetuiamo un assegnamento, ciò che succede è la **valutazione** dell'espressione (che risulterà in un numero) che viene poi assegnato alla variabile. Questo significa che x = x + 1 è un'operazione valida se x è già esistente e ha già una valore (ex. x = 2 -> x = x + 1 -> x = 2 + 1 -> x = 3)

### Rappresentazione di un programma nella memoria di un computer
Si immagini una serie di blocchi, uno sopra l'altro, contenenti delle informazioni circa le operazioni da eseguire e un indirizzo univocamente associato ad ogni operazione.

Quando si esegue un programma è necessario avere uno strumento che ci dica a quale indirizzo di memoria corrisponde ogni variabile, e questo è noto come **ambiente**. Con questa conoscenza, possiamo modificare la definizione di stato in "un insieme di variabili (detto *memoria*) e un ambiente".

## Dichiarazioni di variabili
Per costruire uno stato dobbiamo **dichiarare** delle variabili.

A livello sintattico, possiamo dichiarare principalmente in due modi una variabile:
* TIPO var; [Dichiarazione semplice nella quale informiamo il computer che esiste una variabile di nome var e di tipo TIPO]
    * Ex: `int km;`
* TIPO var = espressione; [Dichiarazione con inizializzazione, nella quale non solo dichiariamo una variabile ma le assegniamo fin da subito il valore di un'espressione]
    * `int km = 10000;`

***NOTA BENE***: ogni variabile che si vuole utilizzare deve essere dichiarata **prima** di poter essere usata e non può essere dichiarata più di una volta.

Inoltre, è importante che ogni variabile dichiarata riceva, prima o poi, un valore (= venga inizializzata) prima di essere usata. Se ciò non avviene, si può incappare in problemi nell'esecuzione del proprio programma.