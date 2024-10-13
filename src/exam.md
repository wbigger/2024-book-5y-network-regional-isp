# Esame di Stato 2024

L'esame di stato del 2024 chiedeva di realizzare una rete regionale per servire strutture pubbliche e convenzionate.

Ecco il primo paragrafo dell'esame.

```text
L’amministrazione di una Regione italiana, attraverso una società appositamente creata, ha
recentemente sviluppato una infrastruttura di comunicazione in fibra ottica, allo scopo di fornire
connettività a banda larga ad Enti locali, scuole e strutture sanitarie pubbliche presenti in tutto il
suo territorio. In particolare, in ambito sanitario, la società gestisce anche un data-center che
raccoglie tutti i dati sanitari dei cittadini residenti in regione, relativi alle prestazioni sanitarie
erogate dalle strutture pubbliche (fascicolo sanitario elettronico). 
```

Cominciamo a fare delle ipotesi aggiuntive: ipotizziamo che la regione sia il Lazio e proviamo a fare delle stime.

- Enti locali: sono i comuni, le provincie, le città metropolitane, etc., nella regione Lazio sono circa 500 (durante l'esame potete chiedere ai commissari per la stima)
- Scuole: considerando tutti gli ordini e gradi, non separando per plesso ma solo per istituto, sono circa 1000
- Strutture sanitarie: considerando solo ospedali e strutture più grandi, sono circa 100
- Data Center: possiamo ipotizzare un singolo data center, localizzato a Roma

Andiamo avanti.

```text
I dati raccolti nel fascicolo sanitario elettronico di ciascun paziente possono essere di vari formati e
dimensioni in quanto riguardano, ad esempio, gli accertamenti diagnostici (es. ecografia), le visite
specialistiche (es. visita cardiologica) e la relativa documentazione (referto, immagini diagnostiche,
video …).
```

Da qui capiamo che il data center dovrà gestire molti file anche di grandi dimensioni.

```text
All’interno della componente M6C2 “Innovazione, ricerca e digitalizzazione del Servizio Sanitario
Nazionale”, prevista dalla Missione 6 del PNRR, la Regione intende estendere la rete in fibra già
esistente, per offrire il servizio di connettività a banda larga a tutte le strutture sanitarie private
convenzionate, in modo che anche i dati da loro prodotti possano direttamente confluire nel datacenter regionale. 
In tal modo, tutti i cittadini ed i medici chiamati a curarli, sia presso strutture sanitarie pubbliche
che presso quelle private convenzionate, avranno a disposizione in un unico luogo virtuale (il
fascicolo sanitario elettronico) tutte le informazioni sanitarie di loro interesse.
```

Qui introduce come dovrà cambiare, che è l'obiettivo principale del nostro compito d'esame.

```text
Per differenziare le diverse tipologie di strutture connesse alla rete (Enti locali, scuole e strutture
sanitarie pubbliche e private), la società regionale che gestisce l’infrastruttura in fibra ha adottato
un piano di indirizzamento utilizzando sottoreti della rete 10.0.0.0/8; in particolare, a questo nuovo
servizio di connettività verso le strutture sanitarie private convenzionate è stata assegnata la
sottorete 10.100.0.0/16. Questa sottorete sarà finalizzata esclusivamente all’interazione con il
data-center delle strutture sanitarie private convenzionate, ma non offrirà loro servizi di accesso
generalizzato ad Internet.
```

Da questo paragrafo cominciamo a capire quali sono i vincoli esistenti sulla rete e come deve essere impostato il lavoro di base del subnetting.

```text
Utilizzando gli indirizzi consentiti da questa sottorete, il progetto dovrà pertanto dettagliare un
piano di indirizzamento che permetta di connettere un numero di strutture sanitarie private
convenzionate che si stima essere intorno alle 2000 in regione (con possibili incrementi futuri),
assegnando a ciascuna di esse la disponibilità di un minimo di 8 indirizzi complessivi.
Ogni struttura sanitaria privata convenzionata ovviamente dispone già di una propria infrastruttura
di rete locale interna. La società regionale di gestione fornirà a tali strutture private convenzionate
un dispositivo per la connessione alla rete regionale, configurato e controllato da remoto dalla
società regionale stessa. Il progetto dovrà garantire che ciascuna struttura collegata non possa
accedere alle reti di tutte le altre strutture connesse alla rete in fibra regionale. 
```

La stima del numero di strutture sanitarie convenzionate è già fornito dalla traccia, insieme al numero minimo di dispositivi per sottorete. Queste informazioni ci saranno molto utili anche per avere un'idea di come impostare la rete pre-esistente.
