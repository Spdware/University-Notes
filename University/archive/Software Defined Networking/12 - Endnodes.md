Inizio capitolo host node: sono i nodi su cui sono presenti le funzioni di compute e hanno una significativa parte di networking. La destinazione del traffico non è esterna al nodo ma è un processo/applicazione in esecuzione sulla macchina locale. Oggi quasi nessun server esegue processi sul OS ma hanno vari strati di virtualizzazione e quindi è presente una parte di networking che instrada i pacchetti verso la corrispondente VM.
Può capitare che intere funzioni sono virtualizzati e quindi un host viene usato come router(Sia lato client che lato server).
Cerchiamo di tenere come riferimento la visione classica poichè gli aspetti virtualizzati sono rappresentati come sistemi di rete con nessuna differenza da nodi di rete reali.

### Caratteristiche
**Funzionale**
Client e server sono identici. Un host che si occupa principalmente da server/client dal punto di vista dei processi, protocolli, standard sono indistinguibili.
Si differenziano per la presenza nella rete: 
- Client è meno potente del server, genera meno traffico nella rete, ha meno crash/cali di performance, si occupa di tutta la parte relativa a comunicazione utente. Ha meno traffico e aprirà meno connessioni verso la rete
- Server: specializzati nella parte di calcolo e comunicazioni, sono molto più carichi e presentano criticità di performance
La differenza consiste negli input e negli obiettivi del progetto.
Entrambi fanno tante cose, dunque sono presenti meccanismi che provocano penalità di performance derivati da:
- Astrazioni usate per semplificare lo sviluppo
- Protezione da errori, attacchi informatici, meccanismi che mettono limitazioni a quello che le applicazioni possono fare, presenti dei meccanismi di verifica che impattano sulle prestazioni
- Si progettano endnodes per essere usati nel maggior numero di endnodes possibili
Come si può ridurre i costi di questi meccanismi?
**Esempi di performance gap**
- Informazioni copiate attraverso diversi domini(possibile ridurre il numero di copie)
- Scelte di progetto che sono ok per basso traffico, ma non scalano per un traffico maggiore(Colpiscono limiti su cache, memoria, etc...)
- Gestione dei timer visto che una connessione TCP si porta dietro diversi timer da dover gestire e ottimizzare(Derivati dallo stato soft di Internet). Gestire un timer è difficile poichè bisogna gestire gli interrupt, bisogna interrompere le operazioni del OS

### Riferimento dell'host con visione HW semplificata

![|400](https://i.imgur.com/HtajWLj.png)

Presentiamo una memoria, una CPU comunicante con la memoria attraverso un bus di dsistema, possiamo avere una cache e una Memory Managment Unit. Presente un bus di IO a cui è collegata l'interfaccia di rete, presente un bus adaptator per adattare i dati sul bus IO al bus di sistema. Presenti poi anche tutte le altre periferiche.
**Commenti sull' host defined networking**
1. Host si basano sule cache, usata per località temporale(stessa zona di memoria letta e scritta in momenti temporali molto ravvicinati/devo leggere e scrivere gli stessi dati più volte) e località spaziale(solitamente accedo a spazi di memoria vicini tra loro quindi è agevole copiarli in memoria). Il problema è che l'elaborazione dei pacchetti presenta una bassa località. Per i pacchetti una volta letto l'header leggo il payload e mi fermo lì.I pacchetti non vanno sempre verso le stesse località. Anche per la località spaziale ho problemi visto che i pacchetti non devono andare sempre verso la stessa direzione(Meno prominente nei client). Dunque il caching non è un risolutore generale di performance.
**Ciclo di vita di un pacchetto in un host**

![|450](https://i.imgur.com/xX89my6.png)

Parliamo di sistemi con un OS che gestisce il dialogo tra i processi che competono per le risorse e la parte sottostante.
La parte di sinistra rappresenta la visione fisica di un host.
La parte di destra rappresenta la visione logica dell'host. La parte in basso rappresenta l'interfaccia di rete, in alto sono presentti i processi utente in esecuzione e in grigio è rappresentato il kernel.
Sono visioni dello stesso sistema, le code sono spazi di memoria, il codice eseguito coinvolge la CPU o il processore dell'interfaccia di rete(firmware). La memoria sarà assegnata ad un processo dal OS.
**Parte Ricezione**
1. Pacchetto arriva dal livello fisico(trasformazione segnali elettrici in bit/byte, livello Fisico/1), consegna degli ottetti al livello 2 attraverso un buffer di ricezione fino a che la trama non è completa.
2. Il livello 2 decide che una trama è completa se trova il delimitatore di trama di fine/inizio. LA trama deve essee completamente ricevuta(Store and Forward). A questo punto il processore controlla la correttezza della trama ricevuta e che sia indirizzata a questo host.
3. Il livello 2 passa la trama al livello 3 presente nel kernel di sistema. Per fare questo la scheda di rete deve segnalare al OS che è presente un pacchetto da dover essere prelevato tramite un interrupt HW. A questo punto il pacchetto deve essere passato al bus di sistema(Parte fisica) e su un coda IP di sistema(Parte logica). Se la coda IP condivisa è piena il pacchetto viene scartato(Macchina troppo carica e non riesce ad elaborare i pacchetti entranti velocemente). Se c'è posto in coda viene messo in coda e se la coda è vuota viene generato un SW interrupt per gestirlo
4. L'host elabora le funzionalità di livello 3 del pacchetto. Presenti meccanismi di filtraggio(caratteristica non essenziale). Si verifica la destinazione del pacchetto: può essere il nodo stesso(situazione maggiormente presente) o un altro nodo(pacchetto in transito). Se il pacchetto è per il nodo locale va passato ai livelli superiori. 
5. Gestione della frammentazione dei pacchetti. Capire quale sia il protocollo di trasporto che viene utilizzato. 
6. Supponiamo di essere su TCP: lo passiamo alla parte di codice che gestisce TCP nel kernel. Cerco il socket corrispondente quardando la porta destinazione:
	1. Socket aperto in listening, dunque su quella porta è presente un server in ascolto. Si procede poi a fare l'handshake(eseguito dal OS, server svegliato al termine dell'handshake per evitare di svegliare un server senza essere sicuri che dall'altra parte ci sia un client).
	2. Arriva il pacchetto verso una porta senza server in ascolto, mando TCP reset
	3. Arriva un pacchetto che non è un CIN/CUp, dunque pacchetto non appartiene a quel socket e viene mandato un pacchetto di TCP reset
	4. Pacchetto per una socket attiva e funzionante viene portato sulla socket queue corrispondente(pago questo costo per mantenere le code separate)
7. Quando entro in una coda TCP verifico i numeri di sequenza e gli acks di processo, verifico che il pacchetto sia in ordine, sveglio l'applicazione se era in attesa di dati, se l'applicazione stava eseguanedo altre operazioni i dati vengono messi in una coda di attesa da cui raggiungeranno immediatamente il processo appena si liberi
8. Sposto i dati allo spazio utente e faccio partire l'applicazione
**Commenti**
- Con data direct access i pacchetti vengono gestiti con interrupt SW solo al completamento del trasferimento del pacchetto permettendo uno scarico della CPU
- Quando il carico è elevato, gli interrupt HW possono far morire di fame gli interrupt SW poichè gli interrupt HW hanno priorità su quelli SW e quindi se arrivano molti pacchetti rapidamente potrebbe accadere che ci siano molti interrupt HW senza che la CPU possa svuotare la coda IP con relativa perdita di pacchetti. La priorità è data al cominciare il lavoro sui nuovi pacchetti al posto dei pacchetti entrati in coda(Receiver LiveLock). Provoca una perdita di pacchetti che corrisponderà a dei timeout e ritrasmissioni(aspetto negativo poichè continuano ad arrivare nuovi pacchetti, per questo esistono meccanismi di congestione che diminuiscono la frequenza a cui i pacchetti vengono ritrasmessi, funziona se le connessioni TCP sono in numero limitato poichè altrimenti non ho il tempo di attivare questi meccanismi). Receiver Livelock è quindi un problema dei server gestito tramite meccanismi TCP con preò grande impatto(non funziona se ho tante connessioni).

### Processo  di un pacchetto in uscita
- Applicazione esegue una system call, copia i dati dallo spazio utente al socket buffer, verifica che sia presente una strada per il destinatario
- Costruisce il TCP header e controlla la finestra di trasmissione del TCP, nel caso non ci sia spazio lo mette in attesa
- Esegue tutti i controlli relativi ad IP e controlla le regole di filtraggio
- Finisce di costruire il pacchetto e lo manda sulla coda di uscita dell'interfaccia di rete. Solitamente l'interfaccia di rete trasmette un interrupt alla corretta spedizione del pacchetto(In caso contrario OS riprova a mandare il pacchetto e in caso negativo trasmette un errore all'applicazione). Toglie il pacchetto dalla coda di trasmissione. Non possiamo avere sicurezza della ricezione del pacchetto a meno che non arrivi il messaggio corrispondente(no da sicurezza che l'applicazione abbia ricevuto il pacchetto, solo che il pacchetto è stato ricevuto dal TCP). 