# LIVELLO DI TRASPORTO

Il quarto livello dello stack TCP/IP, collocato tra il livello applicativo e quello di rete, si occupa di fornire ai processi che lavorano a livello applicativo un'astrazione di comunicazione diretta, generalmente detta anche **comunicazione logica**.

Processi in comunicazione logica possono quindi risiedere su host distanti ma dialogare come se fossero vicini, senza doversi preoccupare della strada percorsa dalle informazioni inviate o del traffico sulla rete. Il livello di trasporto riceve quindi messaggi di tipo applicativo da processi differenti e li inoltra su un unico canale di comunicazione (azione di **multiplexing**). Specularmente, in fase di ricezione estrae dal canale e smista verso i rispettivi processi destinatari (azione di **demultiplexing**). Le socket hanno un ruolo centrale in queste due fasi visto che permettono di identificare univocamente i processi e fanno da ponte tra gli host e la rete.

Fatta questa premessa, possiamo facilmente capire perché i protocolli di livello di trasporto sono implementati esclusivamente sui sistemi periferici, quindi secondo una struttura **end-to-end**.

Ci riferiamo ai messaggi che circolano a livello di trasporto come **segmenti**. In alcune descrizioni del protocollo UDP i segmenti vengono anche chiamati **datagram** (la costante `SOCK_DGRAM` è utilizzata in fase di creazione delle socket per specificare che la socket dovrà essere di tipo UDP). Il termine può inizialmente risultare un po' fuorviante visto che datagram è anche il nome con cui si indicano i messaggi a livello di rete.

### Scelta del protocollo a livello di trasporto: overview

Come accennato nella sezione sul livello applicativo, la scelta di un protocollo a livello di trasporto è fortemente basata sulle esigenze dell'applicativo. Vediamo di seguito i due principali protocolli, che ritroviamo abitualmente nei sistemi moderni, nella loro forma pura o con piccole variazioni. 

## Protocollo UDP

Definito nell' [RFC 768](https://datatracker.ietf.org/doc/html/rfc768), UDP è un protocollo minimale che non offre alcun tipo di garanzia in termini di affidabilità, timing, sicurezza o di qualsiasi altro requisito e, contrariamente a TCP, non mette a disposizione alcuni tipo di servizio per il controllo di flusso o di congestione. Ogni qual volta il livello applicativo richiama UDP questo non fa altro che prendere il datagram e inoltrarlo sulla rete e, in fase di ricezione, effettuare il demultiplexing verso il livello superiore. Non essendoci una fase preliminare per determinare lo stabilimento della connessione tra gli host UDP rientra nella categoria dei protocolli **connection-less**. Nella rappresentazione dei datagram UDP è presente anche un header di checksum. Il checksum è tipicamente una somma in complemento ad uno calcolata sull'intero datagram, a gruppi di 16 bit.

![udp-segment-structure](./img/udp-segment-structure.png)

Nel caso in cui si stia utilizzando il protocollo IPv4 a livello di rete, il calcolo del checksum **non è mandatory** (obbligatorio), lo è invece nel caso di IPv6 visto che quest'ultimo non lo prevede al fine di mitigare la ridondanza dei dati. Che il checksum venga o meno calcolato l'header è comunque presente. Se si sceglie di non implementare il checksum il campo viene posto a zero, altrimenti UDP ricostruisce il cosiddetto ***pseudo-header*** basandosi sui dati forniti dal livello IP. Questo viola la struttura a livelli indicata nello stack ISO/OSI che, come preannunciato, TCP/IP non segue rigidamente.

L'RFC non specifica il da farsi in caso di ricezione di un segmento errato, generalmente viene scartato senza ulteriori segnalazioni al mittente. In *Computer Networking: A Top-down Approach di James F. Kurose*, cap. 3 è comunque possibile ritrovare la costruzione step-by-step di un ipotetico protocollo per il trasferimento dati affidabile, partendo da uno scenario simil-UDP.

Viste le premesse UDP potrebbe sembrare un protocollo da evitare il più possibile, il che non è completamente vero.
Venendo a mancare le garanzie sopra citate si elimina una fetta enorme di complessità dal protocollo, ciò si traduce in **overhead minore**, **dimensione dei pacchetti ridotta** e in una certa **velocità nell'inoltro** sul canale. La struttura minimale di UDP fornisce inoltre agli applicativi un maggiore controllo della comunicazione, che passando i propri messaggi a livello di trasporto possono già darli per inviati, senza preoccuparsi di eventuali modifiche e bufferizzazioni. 

Alcuni protocolli applicativi che si appoggiano ad UDP:

| Procotollo applicativo                     | Ambito di utilizzo                  |
| ------------------------------------------ | ----------------------------------- |
| Domain Name System (DNS)                   | Associazione dominio - IP           |
| Simple Network Management Protocol (SNMP)  | Informazioni sullo stato della rete |
| Routing Information Protocol (RIP)         | Informazioni sullo stato della rete |
| Dynamic Host Configuration Protocol (DHCP) | Assegnazione indirizzi IP           |

## Protocollo TCP