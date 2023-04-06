Index: [[Index]]
Software Defined Networking: [[Software Defined Networking]]

---
Identificazione dei pacchetti tramite più campi dell'header e come modificare i pacchetti tramite questi campi(Serve per abilitare il NAT, attività di load balancing...)
Elenco dei campi in OpenFlow su cui fare match è fissato:
- Ethernet
- MAC (destinazione, sorgente)
- IPv4(Sorgente, destinazione, trasporto)
- UDP
- ARP
Una funzione OpenFlow utile è il parsing(Esegue il match di pacchetti già predeterminati)
Attenzione che alcuni campi esistono solo se il pacchetto appartiene ad alcuni protocolli(Posso capire a che protocollo appartiene il pacchetto da un campo esterno)
Conseguentemente alcune regole di match si possono eseguire solo se i valori di alcuni campi sono corretti
Alcuni match sono consentiti se e solo se la regola comprende altri match che confermino che la regola sia del formato giusto(Sono regole che esprimono prerequisiti).
OpenFlow assume che l'interfaccia sia ethernet e quindi il campo eth_type esiste sempre(Sono consentiti 2 prerequisiti visto che esistono solo pacchetti IPv4 e IPv6)
I vari campi su cui vado a fare match possono essere correlati e quindi devo superare match specifici per avere determinati pacchetti
##### Azioni:
- Set Field: sovrascrive un campo del pacchetto mettendo un valore diverso(In OpenFlow questo valore deve essere una costante), il nome del campo è uguale a quello del match

### Laboratorio
Guarderemo una rete a stella con 3 host(h1,h2,h3)
Scrivere le regole per mappare tutto il traffico verso h2 dalla porta 80 alla porta 8080(e viceversa)
Per concludere la comunicazione correttamente lo switch deve tradurre la porta di uscita sia all'andata che al ritorno
Comando nc permette di implementare velocemente una connessione TCP(posso anche usare iperf)
Azione di output rimane ma dobbiamo aggiungere un'azione di SetField
L'ordine delle istruzioni è importante: l'output deve essere sempre l'ultima azione effettuata
Le regole di match si scrivono come in precedenza stando attenti ai prerequisiti