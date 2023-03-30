SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
### Funzionamento del controllore
Il controllore si interfaccia con gli switch ed implementa il protocollo Openflow e implementa anche degli API
(Prima iterazione)Il piano dati è dotato di un parser leggente i pacchetti in arrivo dove individua gli header e gli indirizza tramite regole già defnite e non programmabili. Definisce un "listone" di campi
Si passò poi alla programmabilità del piano dati(parser flessibile)
Noi usiamo Openflow 1.3

#### Gestione Switch
Lo switch viene astratto come successione di tabelle(anche se noi ne useremo una sola) 
Le tabelle non hanno un unico campo su cui fare match ma lo fanno su più match contemporaneamente
![[Pasted image 20230315155120.png]]
Il match può essere completo o può esserci ambiguità
le ambiguità sono risolte tramite un valore di priorità(il valore maggiore indica il match da scegliere)
Presenza di contatori che aumentano all'aumentare dei match registrati per la riga corrispondente
Sono presenti anche dei Time Out per cui una regola viene rimossa allo scadere del timer(Solitamente alcuni minuti)
La ricerca è esatta su tutti i campi ad eccezione di un campo completamente non specificato
Il risultato corrisponde ad un'unica riga che ha la massima priorità
Se ci sono ambiguità tra regole lo switch si rifiuta di inserire l'ultima regola specificata(Non ben specificato dallo switch)
Può esistere una regola di default(Table miss entry) e corrisponde all'accettare qualunque valore in tutti i campi e con priorità 0 (Se non presente allora il pacchetto potrebbe non essere accettato)
Ogni volta che mandiamo/riceviamo un pacchetto gli viene assegnato anche un numero di interfaccia
- Porta FLOOD: indica tutte le porte dello switch tranne quella da cui è arrivato
- Porta IN: indica la porta da cui è arrivato il pacchetto
- Porta Controller: il pacchetto deve essere diretto/arriva al controller

### Istruzioni
- Apply Action< Lista Azioni >: esegue le azioni immediatamente sul pacchetto
- Goto Table< id tabella >: invia il pacchetto alla tabella specificata, per evitare un loop non posso mandare un pacchetto ad una tabella con numero d'ordine minore 

### Azioni
- Output< porta>: invia il pacchetto alla porta
- Drop: se il pacchetto non ha azioni/istruzione Output specifiche viene droppato
- Set Field< campo> < valore>: sovrascrive un campo del pacchetto e il relativo checksum

### Modelli di Controllo
##### Modello proattivo
Accendo lo switch e il controllore installa tutte le regole necessarie prima di ricever il primo pacchetto
Se necessario scarico altre regole successivamente
##### Modello Reattivo
Non carico regole se non di servizio, quando arriva un pacchetto che lo switch non sa instradare chiede al controllore che scaricherà la regola necessaria
Introduce ritardi nel dialogo Switch Controllore molto elevati, dunque usato in casi particolari

# RYU
Controller Openflow programmabile
Possiede un'interfaccia southbound completamente programmata
Possiede un'interfaccia northbound molto limitata
Bisogna scrivere cosa succede nella comunicazione tra Switch e Controller
![[Pasted image 20230315161012.png]]
Bisogna definire la fase di Handshake
L'unica azine manuale da fare sullo Switch è dirgli l'indirizzo IP del Controllore
Switch sono come client e il Controllore è come un server
###### Setup Iniziale
Switch e Controller si scambiano delle capability: lo switch si collega e comunica il proprio identificativo e come è fatto(# interfacce)
Il controllore può anche fare operazioni di Flow Mod in cui comunica delle regole allo switch
### Main
Switch comunica con il controller solo quando non sa cosa farci
lo switch manda il pacchetto al controllore che gli comunicherà poi cosa farci
In un modello proattivo non si usa il Packet IN ma da il caricamento di altre regole viene attivato da altri input
### Dead
Fase in cui cade la connessione TCP per cui lo switch resetta alcuni parametri e prova poi a riconnettersi tramite un nuovo HandShake

## Esercizio
Topologia a stella con 3 host, realizzare un hub
Programmeremo RYU tramite un programma python
Dovremo inserire una regola match any; action output flood durante la fase di CONFIG_DISPATCHER. La priorità da settare è 1(Non usare 0 poiché gestito differentemente dagli switch)
La funzione nel programma viene eseguita alla configurazione(iniziale) dello switch poi il controllore non serve più se non nel caso in cui lo switch  crashi 