# STACK DI RETE

L'informatico e professore *Andrew Tanenbaum*, autore di *Computer Networks* , descrive il modello ISO/OSI come fondato su tre concetti tra loro **indipendenti**: 

- i servizi
- le interfacce
- i protocolli

In linea con una visione *ad oggetti*, rendere indipendenti questi tre concetti risulta estremamente utile.

Quando parliamo di **servizi** intendiamo ciò che il livello sottostante offre a quello sovrastante ma non specifichiamo il modo in cui questi due debbano relazionarsi. Ci stiamo quindi limitando a definire la semantica del livello.
Le **interfacce** del layer invece definiscono uno standard secondo cui è possibile accedere al servizio offerto. L'interfaccia specifica i nomi dei metodi e gli eventuali parametri da fornire per usufruire del servizio, non fornisce però informazioni riguardo ai meccanismi interni. Infine, il **protocollo** definisce il modo in cui si opera all'interno del layer per fornire il servizio. In una struttura così organizzata cambiare uno o più protocolli non ha effetti sul resto dello stack e il comportamento osservabile rimane inalterato. TCP/IP invece, come vedremo, non si adatta rigorosamente a questa definizione.

La progettazione rigorosa che troviamo nel modello OSI dipende dall'assenza di bias. Essendo stato pensato prima dell'invenzione di molti dei protocolli di reti doveva avere una struttura quanto più generica possibile, che potesse adattarsi alle specifiche di qualunque protocollo. Partendo da questo assunto è stato però molto difficile definire quali funzionalità andassero inserite in quale livello. TCP/IP invece nasce come collector di protocolli già esistenti e quindi è fortemente improntato ad essi. La possibilità di scegliere un protocollo piuttosto che un altro quindi rimane ma è ridotta ai protocolli per cui lo stack è stato designato, di conseguenza il modello non è adatto a descrivere reti troppo diverse da quelle che conosciamo.

Un'altra importante distinzione tra i due modelli è il tipo di connessioni supportate.
Se OSI ammette sia connessioni comunicazioni *connection-less* che *connection-oriented* a livello di rete TCP/IP invece è strettamente *connection-less* per quanto riguarda il networking. Al contrario, a livello di trasporto OSI pretende una comunicazione di tipo *connection-oriented* mentre TCP/IP le ammette entrambe.

Ultimo ma non meno importante dettaglio, TCP/IP si differenzia da OSI anche per il numero di livelli. Gli ulteriori due livelli di sessione e presentazioni previsti da OSI di fatti si sono dimostrati non molto sfruttati dagli applicativi, se non per un set ristretto di funzioni, motivo per cui ad oggi a livello applicativo esistono feature quali ad esempio i *cookie*.

<img src="/home/makapx/Scrivania/Università/Appunti/img/osi-tcp.png" alt="osi-tcp"/>



