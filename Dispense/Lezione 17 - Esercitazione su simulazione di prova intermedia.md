# Lezione 17 (venerdì 27 ottobre 2017)
## Esercizio 1
Si dimostri se è regolare o meno il linguaggio definito dalla grammatica sull'alfabeto Σ = {a, b, c} con categoria sintattica iniziale S e con le seguenti produzioni:
* S -> abS | Sab | aXb
* X -> ccX | cc

### Risoluzione
A primo impatto, ci aspettiamo che la grammatica sia regolare in quanto non ci sono dipendenze strane di lettere da altre. Infatti, il linguaggio definito è una stringa di un qualsiasi numero di ab a sinistra (anche 0), un qualsiasi numero di ab a destra (anche 0) e un numero pari di c (almeno 2) contenuto fra una a ed una b. Per dimostrarne l'aspettata regolarità, proviamo a costruire un ASFND:
* Σ = {a, b, c}
* Q = {0, 1, 2, 3, 4, 5, 6}
* S = 0
* F = {5}
* δ = {<0, a, 1>,
       <1, b, 0>,
       <0, a, 2>,
       <2, c, 3>,
       <3, c, 4>,
       <4, c, 3>,
       <4, b, 5>,
       <5, a, 6>,
       <6, b, 5>}

Il linguaggio è dunque regolare.

## Esercizio 2
Si dimostri se il seguente linguaggio è regolare o meno e se ne dia una grammatica: L = {a<sup>m</sup>b<sup>k</sup>c<sup>t</sup>, m,k,t > 0 AND m <= k + t}.

### Risoluzione
L'idea è che dato che m non è limitato, non possiamo conservare l'informazione di quanto grande sia e di conseguenza non possiamo controllare che k+t sia effettivamente maggiore o uguale di m. Ci aspettiamo dunque che L sia irregolare, e proviamo a dimostrarlo con il Pumping Lemma.

Prendiamo w=a<sup>n</sup>b<sup>n</sup>c<sup>n</sup> che possiamo poi spezzare in w=xyz, con x=a<sup>t</sup> (con 0 <= t < n), y=a<sup>s</sup> (con 0 < s <= n-t), z=a<sup>n-t-s</sup>b<sup>n</sup>c<sup>n</sup>. A questo punto, xy<sup>i</sup>z è uguale a a<sup>t+i\*s+n-t-s</sup>b<sup>n</sup>c<sup>n</sup>. Si dimostra molto facilmente che esiste una i tale che t+i\*s+n-t-s=n+s\*(i-1) > 2n, ad esempio con i > n/s + 1. L è dunque irregolare.

Per trovare una grammatica, dobbiamo pensare, logicamente, a come è costruito il linguaggio. Possiamo utilizzare la solità dualità equalizzatore-destabilizzatore per risolvere il problema: dato che dobbiamo accertarci che il numero di a sia sempre minore della somma delle b e delle c, ci basta far sì che ogni volta che aggiungiamo una a stiamo aggiungendo anche una b o una c. Abbiamo quindi:
* S -> aSc | aXc | Sc [funge sia da equalizzatore che da destabilizzatore: ogni volta che aggiungiamo una a aggiungiamo anche una c, ma possiamo anche aggiungere c senza mettere delle a]
* X -> aXb | Xb | ab | b [come sopra, aXb aggiunge una b ogni volte che mettiamo una a, mentre Xb e b sono dei destabilizzatori]

## Esercizio 3
Si definisca una funzione C che, dato un array a di dimensione dim, restituisce il valore di verità della seguente: esiste i appartenente a [0, dim) tale che a[i] = #{j | j appartiene a [0, dim) AND a[j] > 0}.

### Risoluzione
Intuitivamente, la formula significa che esiste un elemento dell'array tale che il numero di elementi con valore positivo dell'array sia uguale all'elemento scelto.

Dividiamo il problema in più parti, partendo dalla somma degli indici.
```
int contaPositivi(int a[], int dima) {
    int count = 0;
    for (int i = 0; i < dima; i++) {
        if (a[i] > 0) count += i;
    }
    return count;
}
```

Ora dobbiamo solo verificare che esista i:
```
int check(int a[], int dima) {
    int i = 0;
    int trovato = 0;
    int num = contaPositivi(a, dima);
    while (i < dima && !trovato) {
        if (a[i] == num) trovato = 1;
        i++;
    }
    return trovato;
}
```

## Esercizio 4
Si definisca una funzione C che, dato un array a di dimensione dim, restituisce il seguente valore: #{i | i appartiene a [0, dim) AND a[i] = somma su j di a[j], per j appartenente a [0, dim) tale che j%2==0}

### Risoluzione
A parole: dobbiamo trovare la somma di tutti i valori di a ad indice pari e vedere quanti elementi in a hanno esattamente il valore trovato.

Definiamo, quindi, la funzione che troverà la somma:
```
int sommaIndiciPari(int a[], int dim) {
    int s = 0;
    for (int i = 0; i < dim; i++) {
        if (i%2 == 0) s += a[i];
    }
    return s;
}
```

Ora dobbiamo solo contare quanti elementi in a valgano la somma trovata:
```
int conta(int a[], int dim) {
    int conta = 0;
    int somma = sommaIndiciPari(int a[], int dim);
    for (int i = 0; i < dim; i++) {
        if (a[i] == somma) conta++;
    }
    return conta;
}