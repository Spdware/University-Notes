Pagina Iniziale: [[Automazione Industriale]]
Indice: [[Index]]

---
Linguaggio grafico che si basa sulla trasposizione di una rete elettrica molto semplice in logica di programmazione(Uso di Contatti e Bobine)
Presenza di ingressi e uscite, sulle uscite posso scrivere e leggere mentre sulle entrate posso solo leggere
### Elementi Base
![[Pasted image 20230317180621.png]]
**Contatti**
Rappresentano gli ingressi e vengono inseriti sulla sinistra del piolo, normalmente un contatto senza una sbarra all'interno rappresenta un Contatto aperto e dunque il bit associato risulta 0 se aperto 1 se chiuso. Il Contatto chiuso è rappresentato con una sbarra all'interno e ha un comportamento inverso rispetto al Contatto aperto
**Bobine**
Rappresentano le uscite e sono presenti alla destra del piolo
si attivano al passare della corrente, quindi il bit associato vale 0 se le condizioni logiche alla sua sinistra non sono verificate, 1 altrimenti
Esistono anche le bobine Set(Mantiene lo stato logico alto anche quando le condizioni di attivazione vengono a mancare) e Reset(Riportano lo stato logico a 0)
**Programma**
Insieme di pioli letti dall'alto verso il basso e da sinistra verso destra
Il flusso di energia in un piolo va solo in una direzione senza possibilità di invertirsi
### Ciclo a Coppia Massiva
![[Pasted image 20230317181633.png]]
Ad ogni ciclo vengono letti gli ingressi ed aggiornate le variabili del programma, viene eseguita la sequenza di pioli e vengono scritte le uscite in accordo con i valori del programma
Il valore delle variabili lette in ingresso rimane costante per tutto il ciclo del programma dunque l'invertire delle istruzioni che leggono e scrivono su un'uscita in sequenza porterà a risultati differenti
## Funzioni Logiche
### NOT
![[Pasted image 20230317182229.png]]
### AND
![[Pasted image 20230317182247.png]]
### OR
![[Pasted image 20230317182304.png]]
### XOR
![[Pasted image 20230317182321.png]]
# Elementi Dinamici
Possibile associare ad un contatto anche una variabile di uscita che insieme all'esecuzione sequenziale e ciclica di uno schema LD permette la creazione di elementi dinamici(o con memoria) e l'identificazione di variazioni di una variabile
Una variabile una volta calcolata non cambia il suo valore fino al ciclo successivo dove potrà essere resettata o mantenuta
![[Pasted image 20230317182932.png]]
![[Pasted image 20230317182953.png]]
![[Pasted image 20230317183011.png]]
![[Pasted image 20230317183024.png]]
# Istruzioni di temporizzazione
![[Pasted image 20230317184247.png]]
Sono compiute o da temporizzatori o da contatori:
Temporizzatore(Tx): se il piolo consente il fluire della corrente conta il trascorrere del tempo fino ad un valore preimpostato(Presenta una soglia, è resettabile), allo scadere del tempo Tx diventa vero. Se il piolo si disattiva prima dello scadere del timer esso si disattiva. IN Tx.acc si può leggere il tempo trascorso 
Un temporizzatore a ritenuta(TxR) continua a contare anche se il piolo si disattiva
Reset del temporizzatore(RES) ferma il temporizzatore e lo inizializza a 0
Contatore ad incremento incrementa il valore solo sul fronte di salita(piolo subisce una transizione falso-vero), Cx.acc contiene il valore attuale del contatore e Cx diventa vero quando il contatore raggiunge il valore preimpostato
Il segnale di reset(RES) riporta a 0 il contatore Cx
![[Pasted image 20230317185206.png]]
### Funzioni e Blocchi Funzione
Istruzioni complesse simili alle funzioni di un linguaggio di programmazione di alto livello vengono racchiuse in blocchi collegati con un piolo e il loro uso è immediato
