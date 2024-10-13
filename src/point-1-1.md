# Punto 1.1: rete pre-esistente

Il primo punto della prima parte chiede quanto segue:

```text
1. sviluppi una descrizione di massima, anche supportata da uno schema grafico,
dell’infrastruttura di rete in fibra pre-esistente (che connette Enti locali, scuole e strutture
sanitarie pubbliche) e di come questa si evolverà per implementare il nuovo servizio per le
strutture sanitarie private convenzionate, con opportune esemplificazioni degli indirizzamenti
IP adottati;
```

In questa soluzione affrontiamo in particolare dettaglio lo studio della rete pre-esistente, per esercizio. Durante il compito, sarà possibile anche essere un po' più superficiali ed affrontare più in dettaglio l'evoluzione della nuova rete.

## Piano di indirizzamento

Usando un approccio **top-down**, partiamo dalla nostra rete di base e dividiamo in macro subnet, in base alle nostre esigenze.

Le subnet che ci servono sono:

- Enti locali
- Scuole
- Strutture sanitarie pubbliche (SSPub)
- Data Center

A queste, dobbiamo aggiungere una subnet per tutti i router di aggregazione e core della rete regionale (vedremo dopo il suo uso in dettaglio).

La rete di partenza è data dal testo ed è `10.0.0.0\8`.

Quanti bit dedichiamo ad ognuna delle categorie sopra? Prendendo ispirazione dalla rete suggerita dal testo per le strutture private, per semplicità possiamo provare ad assegnare a tutte le categorie un `/16`.

| Nome             | Indirizzo     |
|------------------|---------------|
| Core/Aggregation | 10.0.0.0/16   |
| Data Center      | 10.10.0.0/16  |
| SS Pub           | 10.20.0.0/16  |
| Scuole           | 10.30.0.0/16  |
| Enti Locali      | 10.40.0.0/16  |
| SS Privato       | 10.100.0.0/16 |

Ora per ogni categoria dobbiamo calcolare quanti bit servono per la singola struttura, in base alla stima fatta precedentemente. Vediamo come fare.

- Data center: il data center al momento è uno solo ma possiamo ipotizzare che ce ne potrebbero essere altri, ma in numero comunque ridotto; per semplicità, possiamo assegnare ad ogni singolo data-center un `/24`, per mantenere un subnetting semplice
- SS Pub: ne abbiamo ipotizzati 100, quindi anche in questo caso per semplicità posso ipotizzare un `/24`, perché tra `/16` della precendente e `/24` ci sono 8 bit, che mi danno la possibilità di 256 reti, ci rientro abbondantemente; inoltre ogni rete avrebbe 8 bit per gli host, quindi $256-2=254$ hosts, che consideriamo sufficienti, anche considerando che il testo ci dice che le strutture private ne devono avere almeno 8
- Scuole: ne abbiamo ipotizzate 1000, qui dobbiamo fare un po' di calcoli. Nel dettaglio, i passi da seguire sono:

1. calcolo la potenza di 2 più vicina al numero di subnet che devo creare, in questo caso 1000 -> **1024**
2. trovo l'esponente della potenza di 2, in questo caso  $1024=`2^10`$ -> 10 bits
3. calcolo la netmask in slash notation, a partire da quella precendente, quindi partendo da `/16`, ottengo $16+10=26$ -> `/26`
4. a questo punto posso calcolare quanti host avrei per struttura: se 26 bit sono bloccati dalla maschera di rete, mi rimangono $32-26=6$ bits liberi per gli host, quindi $2^6=64$ indirizzi IP, a cui tolgo due indirizzi per rete e broadcast, ottenendo 62 indirizzi per ogni scuola. Anche qui, ritengo il numero accettabile e confermo la maschera `/26`.

- Enti Locali: ne abbiamo stimati circa 300, ripetendo il calcolo di prima ottengo un `/25` con 126 host disponibili, anche in questo caso ritengo le stime accettabili

- SS Private: sono circa 2000, ripetiamo nel dettaglio il calcolo:

1. calcolo la potenza di 2 più vicina al numero di subnet che devo creare, in questo caso 2000 -> **2048**
2. trovo l'esponente della potenza di 2, in questo caso  $2048=`2^11`$ -> 11 bits
3. calcolo la netmask in slash notation, a partire da quella precedente, quindi partendo da `/16`, ottengo $16+11=27$ -> `/27`
4. a questo punto posso calcolare quanti host avrei per struttura: $32-27=5$ bits liberi per gli host, quindi $2^5=32$ indirizzi IP, a cui tolgo due indirizzi per rete e broadcast, ottenendo 30 indirizzi per ogni struttura.

Possiamo notare che abbiamo molti più hosts per struttura di quanti richiesti dal testo (30 rispetto ad 8), mentre siamo abbastanza stretti nel numero di strutture che possiamo supportare, in quanto il massimo è 2048, ma il testo ci chiedeva di prevedere incrementi futuri. Ci conviene quindi usare un bit in più per le reti, passando da `/27` a `/28`, in modo da supportare fino a 4096 strutture, togliendolo dagli host, quindi $2^4=16$ IP -> 14 hosts, che sono comunque accettabili.

