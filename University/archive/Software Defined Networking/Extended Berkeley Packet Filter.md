SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
Evoluzione di BPF in direzione di renderlo uno strumento più efficace anche per elaborazione di pacchetti nello stack di rete saltando il livello applicativo.
Parte dal classic BPF e si basa su funzionalità del kernel linux. Utilizzata per processo di pacchetti in rete, avere approcci a runtime più veloci delle normali tecnologie.
Si parte da applicazioni single tasked a multitasking alla virtualizzazione(applicazione impacchettata nella VM e poi spostata tra i vari host-server). Ora si è passati a servizi cloud, spezzando i servizi in microservizi. 
Necessari overhead limitati(Ex: 5G deve avere una latenza molto minore delle applicazioni precedenti), le applicazioni devono girare all'interno di server linux(Server general purpose), le applicazioni devono avere uno sviluppo agile/veloce, permettono di produrre una funzionalità in brevissimo tempo, le applicazioni devono coesistere con gli altri xservizi presenti all'interno del server.
Due possibilità per realizzare applicazioni:
- All'interno User Space: sono applicazioni che girano all'interno del US e aggirano il kernel poichè possono accedere direttamente ai pacchetti. Vantaggiose poichè forniscono un throughput elevato(Non processo le operazioni del kernel). Svantaggiose poichè consumano molte risorse e richiedono dei moduli kernel dedicati e difficili da integrare con applicazioni native.
- All'interno del Kernel Space: sono applicazioni che girano all'interno del kernel, integrate nel OS. Svantaggiose poichè hanno un impatto maggiore sulle performance. Vantaggiose poichè il processo di sviluppo è stabile.
Con eBPF abbiamo permesso di attuare applicazioni kernel senza dovere ricompilare il kernel. Uso VM per inietttare funzionalità a runtime nel kernel linux. Vantaggiose anche perchè le applicazioni possono interagire con il kernel subsystem, posso chiamare anche funzioni che fanno parte del kernel. Le applicazioni che iniettiamo devono essere sicure e semplici/veloci(Non deve essere possibile leggere/modificare informazioni sensibili, il rispetto di questi parametri viene esposto da un controllore all'interno del kernel). Le performance dei programmi BPF sono migliori dei programmi a livello kernel.
Utilizzi:
- Monitoring nel kernel
- Processamento dei pacchetti
Grazie a eBPF è possibile saltare parti del kernel linux a vari livelli/quantità(Posso avere più punti di attacco del eBPF).
Permettono anche di arginare gli attacchi DDoS poichè permettono anche di accelerare il drop dei pacchetti.
Funzioni eBPF:
- Shortcut di parti kernel linux
- Utilizzo di altre funzioni kernel linux
### Storia di eBPF
Nasce come estensione del classic BPF. Considerato come una CPU virtuale che legge valori di memoria ed esegue applicazioni in base ai valori letti.
eBPF introdotto estendendo l'architettura BPF permettendole di eseguire anche programmi più complessi di filtraggio pacchetti.
Oggigiorno il classic BPF non esiste più, è stato soppiantato dal eBPF(oggi è questo quello considerato quando si parla di BPF).
### Funzionalità principali
##### Iniezione di programmi nel kernel
Permette di saltare tutto il passaggio di modific del kernel, lavoro che richiede anni per essere effettuato. Ora da un'attesa di anni si è passati all'attesa di qualche giorno/settimana.
Permette anche di modificare il codice sul momento, posso adattarmi ai pacchetti che sto ricevendo(richiesto un compilatore di software automatico).
##### Sicurezza
I programmi iniettati devono essere sicuri visto che vanno ad essere eseguiti in modalità privilegiata. Per permettere ciò i programmi eBPF sono eseguiti in delle sandbox dove il kernel verifier verifica che il codice inserito non legga ciò che non può, abbia durata limitata. Per questo i programmi eBPF non possono supportare tutte le tipologie di programmi(anche se eBPF sono Turing-Complete). 
##### Efficienza
VIsto che i programmi sono eseguiti in kernel space il codice compilato può essere sia just-in-time compiled sia interpretato. Il codice BPF è scritto in c, viene compilato in eBPF bytecode e poi viene convertito in istruzioni native della macchina(non ho overhead per l'esecuzione del codice eBPF). 
##### Possono reagire ad eventi kernel generici
Il codice eBPF può essere attaccato in diversi punti del kernel, permettendo di reagire a diversi eventi come la ricezione di un nuovo pacchetto, dato scritto sul disco... 
##### eBPF Hook point
DIversi punti di attacco del codice eBPF, può essere attaccato anche su più livelli(XDP, TC, SK-SKB...)
XDP è il livello più basso e mi da le prestazioni migliori
TC si trova a livello NetFilter dove posso riutilizzare funzionalità del kernel che ho già estratto, ma pago in prestazioni.
##### eBPF Helpers
Funzioni che fanno parte delle API dei eBPF che permettono di riutilizzare le funzioni del kernel
##### Persistent storage
eBPF adotta una memoria preformattata invece di utilizzare pagine di memoria base non strutturate. Sono passato ad essere programmi stateful. Chiamate eBPF maps. Sono realizzate attraverso diverse strutture dati, possono essere condivise tra programmi eBPF e tra programmi eBPF e programmi in User Space
##### Service Chains
Posso iniettare più programmi nel kernel ed è possibile saltare tra questi programmi.
##### Portability
Posso scrivere il programma eBPF per una tipologia qualunque di nodo di rete, non devo riscrivere una funzione per ogni struttura HW
### Come creare programmi in eBPF
Possono essere scritti tramite C(anche se ho alcune limitazioni).
Viene poi compilato da un compilatore che crea un codice intermedio per poi tradurlo in assembly. Il programma viene a questo punto mandato al corrispondente compilatore.
Una volta all'interno del kernel possiamo attaccare il codice ai diversi hook.
# Il futuro del Network Computing
La legge di Moore dettava l'avanzamento tecnologico dei microchip. Una legge simile è quella di Dennar(Dennar Scaling). Dice che, cisto che i transistor diminuiscono di dimensione, posso inserire più transistor nella stessa area senza aumentare il consumo di potenza. Oggigiorno queste leggi non reggono più visto che stiamo raggiungendo dei limiti fisici che non possiamo superare. I processori non riescono più a gestire il traffico in arrivo ai  server, non si riescono a realizzare CPU abbastanza potenti. Si sta cercando di implementare alcune funzionalità tramite HW specializzato in modo da ridurre il carico lato SW. I SHC sono delle FPGA o Smart NICs programmabili che permettono di ottimizzare molto   alcune operazioni sfruttando la velocità HW.
L'uso di elementi HW specializzati porta però a dover risolvere alcuni problemi di connessione/trasformare programmi eBPF in programmi accettabili da componenti HW.
Il corso magistrale si chiama **Network Computing(058172)** dove si esplorano hardware programmabile, studio per il networking di datacenter.
L'importanza del eBPF è stata notata da diverse grosse aziende.