SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
# Layered Demultiplexer
Pacchetto in entrata è analizzato a strati protocollo per protocollo. Approccio dispendioso, richiede molte operazioni e molte copie del pacchetto tra memorie. Si cerca allora di saltare parti di demultiplazione per accelerare i tempi ma così viene persa tutta la parte gestita dal OS(può non servire o è gestita da noi nell'applicazione).

![|200](https://i.imgur.com/yaZlAfT.png)

Decisioni vengono prese in base alle varie intestazioni dei pacchetti. Vantaggioso poichè separa le funzioni e passa da uno strato all'altro tramite punti di accesso ben definiti, protezione dei domini all'interno dell'host.
Approccio usato da decenni con crescita significativa negli ultimi anni grazie alla virtualizzazione dei server e delle reti(Demultiplazione a strati tra le varie virtualizzazioni portava a una mole di operazioni eseguite senza motivo).
# Berkeley Packet Filter
Strumetno più usato per gestire demultiplazione veloce. Si può eseguire la scelta fatta in demultiplazione a strati subito leggendo l'intero pacchetto subito consegnando il pacchetto direttamente all'applicazione. Vantaggio in prestazione perdendo i vantaggi di protezione, astrazioni dati dalla demultiplazione a strati.
Vantaggi: 
- posso implementare protocolli senza modificare il kernel dell'OS(uso di codice specializzato)
- posso dare priorità ad alcuni pacchetti
- posso consegnare i pacchetti saltando tutta la gestione del OS
L'applicazione però deve gestire tutte le astrazioni e protezioni che sono state tralasciate(non sempre viene fatto per non duplicare codice/si vuole implementare algoritmi che permettono di avere grossi vantaggi rispetto a quelli standard).

# Demultiplazione Anticipata
Si può fare in molti modi ma presenta il problema che deve riconoscere i pacchetti in entrata. Negli host il meccanismo di parsing deve essere definito poichè si vuole più flessibilità. Lo strumento più comune usato per questo tipo di ricerca è il Berkeley Packet Filter e implementa un meccanismo a controllo di flusso: il pacchetto arriva, lo si legge tramite un linguaggio di programmazione a basso livello interpretato da una pseudo macchina che legge i campi del pacchetto li confronta con delle costanti e prende decisioni(Grafo di decisione)

![|200](https://i.imgur.com/C8ZWrpM.png)

Il BPF è un insieme di funzioni implementate nell'OS.

![|300](https://i.imgur.com/RKDqqZU.png)

Come nell'esempio sopra si può vedere come un pacchetto venga mandato sulla coda del TCP/IP generando un interrupt. BPF si inserisce in questo meccanismo, quando attivo, poichè il pacchetto viene mandato anche a BPF(fatta una copia). Qui viene letta la copia dal filtro leggendo i vari campi del pacchetto con costanti e prende una decisione: va consegnato ad una applicazione o no. Se si lo consegna altrimenti scarta la copia(l'originale viene consegnato all'applicazione relativa). I filtri sono installati dall'applicazione interessata all'early demultiplexing.
rarpd è il nonno di DHCP(mandava una richiesta per conoscere il proprio indirizzo ip quando un host si svegliava). Serve per gestire pacchetti che non hanno campo TCP e sarebbero scartati. Altra applicazione è quella di un server che implementa un TCP modificato(cosa che non si può fare se non ricompilando il kernel del OS, tralaltro modifico il TCP pre tutte le applicazioni). Se un pacchetto deve andare siìolo da BPF devo sopprimere la copia che non va a BFT tramite dei protocolly di policy-controll 
La copia è necessaria poichè se sono presenti applicazioni di monitoraggio necessitano una copia del pacchetto poichè soffierebbero il pacchetto all'applicazione di riferimento altrimenti. Questo anche per l'altra applicazione poichè non si può avere un'applicazione che controlla il campo ether_type dei pacchetti entranti. Quello che posso fare è farmi dare i pacchetti grezzi e poi elaborarli da solo(rarpd). Qui la cosa è ancora più complicata poichè non legge pacchetti con indirizzo della macchina server corrispondente. 
BPF è utile per monitoraggio passivo, implementazione di nuovi protocolli o per applicazioni che implementano varianti dei protocolli noti.
*Esempio: come funziona TCPdump*
tcpdump "host 131.175.6.1"(chiede al OS di avviare un filtro BPF)
BPF lancia un filtro catturanti pacchetti per/da 131.175.6.1. BPF instanzia il filtro e un buffer utile a non consegnare subito il pacchetto poichè i filtri hanno priorità altissima, non usabili per mandare un pacchetto all'applicazione con priorità troppo alta. Applicazione abilita la lettura dei pacchetti dal buffer(ritiro totale periodico). BPF controlla tutti i filtri attivi sul pacchetto in arrivo(non è possibile combinarli in un superfiltro, solitamente non è un problema poichè ci sono solitamente pochi filtri attivi).
Osservazione importante: le applicazioni eseguono un'operazione molto pericolosa: caricano un programma del OS che legge i pacchetti ad alta priorità e li filtra(Devono avere permessi amministrativi).
L'interfaccia solitamente usata si chiama pcap. 
I pacchetti arrivano dall'adattatore di rete e se si vuole BPF si implementa nell'adattatore di rete un ignoramento dell'indirizzo MAC. Inoltre si adattano anche per catturare tutti  i pacchetti indirizzati ad altri host.

## Filtro BPF
Filtri scritti a bassissimo livello assimilabile ad assembly eseguiti su una BPF machine, oggigiorno BPF viene compilato in assembly/linguaggio del processore nativo.
BPF machine consiste in:
- Accumulatore: registro contenente bit letti 
- Registro indice
- Memoria dedicata
- Program counter: indica dove sono arrivato nella lettura del programmma descrivente il filtro, può andare solo in avanti non può essere decrementato. Due conseguenze: 1. possibile dimostrare con certezza che il programma finisca, dunque il filtro termina 2. BPF non può essere Turing complete(Classic BPF)
Serie di istruzioni:
- Operazioni di caricamento su accumulatore/registro indice da:
	- valore immediato
	- dati da pacchetto
	- dimensione pacchetto
	- memoria
- Lettura dati da accumulatore/registro indice in memoria
- Istruzioni ALU
- Istruzioni di salto:
	- Solo in avanti
	- Condizionato
	- Incondizionato
- Return: ha sempre un argomento consistente nella quantità di byte da copiare nel buffer(se il pacchetto è più piccolo viene copiato tutto)
- Operazioni di miscellanea

![|500](https://i.imgur.com/2hbhjUV.png)

**Modi di indirizzamento**

![|400](https://i.imgur.com/eTubOR4.png)


