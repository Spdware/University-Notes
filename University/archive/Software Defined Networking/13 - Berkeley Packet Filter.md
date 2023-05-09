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

Come nell'esempio sopra si può vedere come un pacchetto venga mandato sulla coda del TCP/IP generando un interrupt. BPF si inserisce in questo meccanismo, quando attivo, poichè il pacchetto viene mandato anche a BPF(fatta una copia). Qui viene letta la copia dal filtro leggendo i vari campi del pacchetto con costanti e prende una decisione: va consegnato ad una applicazione o no. Se si lo cnsegna altrimenti scarta la copia(l'originale viene consegnato all'applicazione relativa). I filtri sono installati dall'applicazione interessata all'early demultiplexing.
rarpd è il nonno di DHCP(mandava una richiesta per conoscere il proprio indirizzo ip quando un host si svegliava).
La copia è necessaria poichè se sono presenti applicazioni di monitoraggio necessitano una copia del pacchetto poichè soffierebbero il pacchetto all'applicazione di riferimento altrimenti.