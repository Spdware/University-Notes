SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
# Switching
Partiamo dalla fine della volta precedente in cui abbiamo inserito nello switch una regola che permetteva di instradare pacchetti di cui si era già analizzata la porta di ingresso/uscita senza passare dal controllore
Dunque se non c'era la costante di FLOOD nella Outport possiamo inserire nello switch una regola che userà comunque Packet_Out ma lo switch potrà andare subito la regola inserita(Indirizzo destinatario: interfaccia Packet_In, Indirizzo destinatario: interfaccia Packet_Out)
Il limite di questa soluzione è che non funziona su topologie con anelli(funziona solo su topologie ad albero)
Non poteva implementare lo spanning tree protocol
Noi però vogliamo instradare i pacchetti in modo corretto senza usare la modalità broadcast
2 condeguenze:
- non posso usare messaggi di ARP request visto che usano messaggi broadcast(Bisogna risolvere indirizzi IP in MAC senza usare messaggi broadcast(Possiamo usare una funzione Proxy-ARP))
- Non possiamo usare un meccanismo di Learning
Temporaneamente ci sbarazziamo del primo problema caricando da piano di gestione in tutti gli host le tabelle di traduzione indirizzo IP-Indirizzo MAC(Fatto da mininet se invocato con parametro --arp), la soluzione più corretta sarebbe usare Proxy-ARP
Concentriamoci ora sul problema numero due:
RYU gestisce questo problema tramite la mappatura della rete(periodicamente il controllore dice agli switch dei pacchetti di discovery che permetteranno al controllore di capire quali switch sono collegati e perciò costruisce la rete, si usa l'opzione --observe-links)
Per farlo dovremo costruirci un grafo rappresentante la rete, ad ogni pacchetto che arriva scopro la strada che percorre e me la segno
RYU conosce un host se ha parlato almeno una volta spontaneamente
Se durante la ricerca dello switch destinazione lo trovo mi fermo e ritorno la sua posizione altrimenti restituisco NULL
Il grafo conterrà SOLO gli switch mentre gli host saranno calcolati in modo diverso
Ora va creato il grafo della rete annotando gli switch e il collegamento delle interfacce, a noi interessa una rappresentazione come grafo diretto(Una coppia di archi per ogni link fisico esistente, sull'arco associamo l'interfaccia fisica che li collega)
Ora viene trovato il percorso minimo per il raggiungimento di ogni switch
Ricostruiamo il grafo della rete all'arrivo di ogni pacchetto(Soluzione semplice ma inefficiente, creo un grafo vuoto e aggiungo tutti gli archi che trovo)
Una volta ottenuto il grafo della rete invoco la funzione shortest path che mi trova il percorso più corto(ricevo una lista di hop di cui mi interessa solamente il next-hop(noi facciamo routing next-hop))
Con net\[s1\]\[s2\] trovo il link da s1 a s2(Unica informazione che serve)
Noi useremo/instraderemo solo pacchetti IPv4 gli altri li scarteremo
Se non si trova lo switch destinazione non possiamo fare niente, altrimenti abbiamo trovato l'host e lo switch a cui è collegato. Due opzioni: lo instrado sulla strada diretta(Dalla packet in deduco che sono sullo switch destinazione) oppure abbiamo un routing indiretto(devo passare tramite routing intermedi) e dunque chiamo la find next switch(Fallisce se la rete è partizionata)
In mininet possiamo testare questa interpretazione con una rete ad anello solo con la topologia torus
Per aggiungere il pacchetto di risposta basta controllare da che interfaccia è entrato il pacchetto iniziale