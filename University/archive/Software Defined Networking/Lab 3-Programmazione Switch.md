SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
# Messaggi OpenFlow
### Messaggio Packet IN
Usato dallo switch per inviare pacchetti al Controller quando l'azione specificata nella tabella Openflow è Output verso il Controller
Usato comunemente quando lo switch non sa gestire il pacchetto e lo manda al controllore pre chiedere come gestirlo
Può essere inviato sia l'intero pacchetto che solo l'intestazione
Il pacchetto viene estratto dallo switch, trasformato in una sequenza di byte, incapsulato in un messaggio applicativo OpenFlow ed inviato al Controllore
### Messaggio Packet OUT
Usato da un controllore per inviare ad uno switch una stringa di byte, tramite messaggio OpenFlow, che esso interpreterà come un pacchetto di rete da mandare in uscita su una certa interfaccia(pesate dal punto di vista computazionale)
Usato anche quando Packet In invia un messaggio che richiede una risposta da parte del Controllore

# Realizzare Switch tramite messaggi Packet In Packet Out
Useremo una tabella di instradamento in cui implementeremo anche il meccanismo di apprendimento
Produrremo il fatto che lo switch manda i messaggi al controllore e che restituirà la porta su cui instradare i pacchetti
Useremo un comportamento reattivo
Regola da inserire sarà  quella match any, action CONTROLLER, NO_BUFFER
Da inserire il prima possibile durante la fase di CONFIG_DISPATCHER alla ricezione di FeaturesReply
Dovremo gestire gli eventi Packet-In durante la fase di MAIN_DISPATCHER(Va dichiarato all'inizio)
Necessitiamo di uno stato, il controllore presenterà questo stato poiché è lui che presenta la tabella di controllo(Va definita una variabile aggiornata da un evento al successivo)
In questa variabile salveremo una tabella associante indirizzi MAC e interfacce
Dovrà anche essere gestito il fatto che il Controllore deve tenere conto di TUTTE le tabelle degli switch nella rete(Gestita come un albero in cui al primo livello è presente una tabella associante all'identificativo dello switch un riferimento alla tabella di quello switch che potrà crescere a necessità)
Allora il Controllore quando interrogato da uno switch manderà la regola relativa allo switch che lo richiede(quando viene scambiata una regola FLOOD con il controllore va anche specificata la lunghezza del pacchetto scambiato)
Ad ogni aggiunta di uno switch sarà creata e associata la relativa tabella che sarà vuota inizialmente
Metadato: informazione relativa al pacchetto
Una volta che il messaggio arriva al Controller estraiamo la porta di ingresso e la salviamo in IN port
Poi facciamo il parsing del pacchetto(Estrazione indirizzo MAC sorgente e Destinatario)
Devo ora scrivere in tabella dove deve andare il pacchetto e poi devo ricostruisco il pacchetto e lo rinvio allo switch
**Parsing del pacchetto**
Attività facilitata da RYU che comprende le operazioni di parsing degli standard più comuni. Uso la funzione Packer(msg.data)
Poi devo dire che mi interessa l'header del pacchetto tramite la funzione get_protocol(ethernet.ethernet)
- Se il pacchetto non è di tipo ethernet la variabile eth conterrà None
- Se il pacchetto è di tipo ethernet avrò una struttura dati contenente tutte le informazioni necessarie
Due possibilità in cui MAC destinazione non in tabella:
- Pacchetto vuole andare in broadcast
- Pacchetto non appartiene alla tabella
Se non è presente in tabella uso come porta di uscita FLOOD altrimenti assegno la porta corrispondente
**Creazione Packet out e invio**
Il messaggio packet out contiene lo switch a cui mandare packet out,  una azione da eseguire
A questo punto lo switch estrae il pacchetto e lo invia alle interfacce di uscita corrispondenti
