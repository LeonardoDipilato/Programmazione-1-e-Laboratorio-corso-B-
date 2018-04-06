# Lezione 21 (martedì 14 novembre 2017)
## Ricorsione
Una funzione ricorsiva, in matematica, è una funzione che chiama se stessa all'interno del suo corpo. Ad esempio, consideriamo la funzione fattoriale: `F(n) = n * F(n-1) se n != 0 | 1 se n == 0`

Questo significa che F(0) = 1 perché per n == 0 F(n) vale *esplicitamente* 1 (detto "caso base"). F(1) = 1 * F(0) = 1 * 1 = 1. F(2) = 2 * F(1) = 2 * 1 * F(0) = 2 * 1 * 1.

Lo stesso concetto può essere portato in programmazione. Infatti, ora come ora, se volessimo scrivere la funzione fattoriale `fact()` utilizzeremmo un approccio iterativo (ovvero basato su dei cicli):
```
int fact(int n) {
    int ris = 1;
    for (int i = 1; i <= n; i++) {
        ris *= 1;
    }
    return ris;
}
```

Se invece volessimo utilizzare un approccio ricorsivo (ovvero chiamando la funzione all'interno di se stessa), dovremmo scrivere qualcosa tipo:
```
int factR(int n) {
    if (n == 0) return 1;
    else return n * factR(n-1);
}
```
Il che risulta in un codice più compatto ed elegante, oltre ad essere una trasposizione immediata della funzione matematica.

## Lunghezza di una lista
### Approccio iterativo
```
int length(ListaDiElementi l) {
    int cont = 0;
    while (l != NULL) {
        cont++;
        l = l -> next;
    }
    return cont;
}
```
### Approccio ricorsivo
```
int lengthR(ListaDiElementi l) {
    if (l == NULL) return 0;
    else return 1 + lengthR(l -> next);
}
```

## Svantaggi delle funzioni ricorsive
Il problema di una funzione ricorsiva è che è facile "romperla". Ovvero, se, ad esempio, chiamo `factR(-2)` il programma inizia un loop infinito (mentre `fact(2)` semplicemente restituirebbe 0, risultato comunque sbagliato). Di conseguenza è ***fondamentale*** assicurarsi che per qualsiasi input all'interno di una funzione ricorsiva si arrivi, prima o poi, al caso base.

## Teoria della ricorsione
Data una funzione f : A -> A si dice **punto fisso** di f un valore x appartenente ad A tale che x = f(x).

Per studiare la teoria dela ricorsione ci concentreremo su un particolare tipo di funzione: le **trasformazione di insiemi**.

Le trasformazione di insiemi sono funzioni che prendono un insieme di valore come input e restituiscono un insieme di valore come output.

Ex.: T(A) = A', A' = {y | y = 2x, x appartenente ad A}. Di conseguenza, T({1, 2, 3}) = {2, 4, 6}

Dunque, ne traiamo che:
> Dato un insieme A di **tutti** i valori possibili, una trasformazione è una funzione T : P<sub>A</sub> -> P<sub>A</sub>, dove P<sub>A</sub> è l'insieme delle parti di A

Usiamo quindi la nozione di punto fisso applicata alle trasformazioni di insiemi per definire insiemi ricorsivamente: X = T(X). Abbiamo, infatti, che X è definito come funzione di se stesso se X è punto fisso per T.ù

## Preliminari per il teorema della ricorsione
### Monotonia
T : P<sub>A</sub> -> P<sub>A</sub> si dice monotona se, dati X, Y appartenenti a P<sub>A</sub> con X sottoinsieme di Y, allora T(X) è sottoinsieme di T(Y).

### Continuità
T : P<sub>A</sub> -> P<sub>A</sub> si dice continua se, data una qualunque sequenza non decrescente di insiemi X<sub>i</sub> appartenenti a P<sub>A</sub> abbiamo che l'unione su i >= 0 di T(X<sub>i</sub>) = T(unione su i >= 0 di X<sub>i</sub>). Se gli insiemi sono ordinati in modo non decrescente, fare l'unione e applicare T al risultato corrisponde ad applicare T ad ognuno di essi e prendere l'unione dei risultati.