# Mininet
Permette di creare reti virtuali con tutte le loro macchine e nodi di rete

Useremo solo l’interfaccia virtuale

Dare in input la descrizione di una topologia e lui creerà la rete corrispondente

Presente anche un controllore poiché gli switch sono solo SDN(implementano solo le funzioni del piano dati) e poi il controllore sarà incaricato di caricare la configurazione sullo switch
Comandi mininet:
- --mac: assegna indirizzi MAC progressivi invece che casuali
- sudo mn: istanzia la rete mininet(i seguenti sono parametri opzionali)
-   --arp: popola staticamente le tabelle ARP di tutti gli host
-   --link tc, bw =<banda in Mbit/s> delay=<Ritardo>loss=<Perdita in percentuale>: indica la velocità dei link presenti nella rete(Se non specifico niente Mininet impone parametri di default limitati dalla potenza della macchina su cui sta girando)
Topologie:
-   --topo{single | linear | tree, param…}
-   minimal: due host uno switch
-   single: topologia a stella
-   linear: topologia lineare
-   tree.depht=<livelli>: topologia ad albero
-   torus: topologia con multipli anelli(attenzione al traffico broadcast, più difficile da usare)
Una volta dato il comando mn apparirà una console di comandi:
-   <nomehost> comando : esegue il comando sull’host
-   pingall: esegue ping tra tutte le coppie di nodi
-   iperf<nome1><nome2>: esegue iperf tra i due host
-   exit: permette di uscire da mininet
Switch SDN fa match non solo sugli indirizzi MAC del pacchetto
Posso così realizzare diverse tipologie di strutture
# Vagrant
Comandi per attivare la macchina virtuale:
-   vagrant up: permette di avviare la macchina virtuale
-   vagrant ssh: permette di collegarsi alla macchina virtuale
-   exit: fa uscire dalla macchina virtuale
### Tipologie di oggetti:
Come realizzare un hub:
Vedremo come creare una topologia a stella o in linea(in questo caso con tre host)
Dobbiamo inserire nello switch una regola del tipo match su tutto e flooding. Bisogna crearsi il proprio controllore. Per lanciarlo basta usare il comando ryu-manager flowmanager/flowmanager.py
