Index: [[Index]]

Automazione Index: [[Automazione Industriale]]

---
Permettono di passare dalle specifiche alle reti di petri.
Possono essere necessarie delle ipotesi per coprire situazioni non chiarite dalla specifica. Non presentano differenze tra la specifica e il codice che implementano.
### Come specificare il comportamento del sistema: approccio funzionale
Elenco le macrofunzioni:
- Attività(OP, LAV): individuazione delle mansioni che deve compiere la nostra macchina 
- Ricetta: operazione in cui si uniscono le varie attività tramite dei posti di transizione(cercare di evitare di aggiungere roba a caso).
- Risorse a Disposizione: comporta l'utilizzo dei token all'interno della RdP 
- Prodotti: sono modellizzati attraverso i token all'interno dei posti. 

# Modelli delle attività a due eventi e tre stati

![](https://i.imgur.com/532XARe.png)

Il modello a due eventi e tre stati modella qualsiasi attività tramite uno stato di inizio, fine e processo. Inoltre usufruisce di degli eventi di inizio e fine per modellare l'azione.
Presenterà stati fisici modellanti condizioni in cui si trovano le componenti del sistema e stati logici modellanti condizioni logiche di funzionamento.
Si passa poi a definire tutte le condizioni necessarie al verificarsi dell'attività(precondizioni, definiranno il preset della transizione di start) e tutte le condizioni che varranno una volta svolta l'attività(postcondizioni, definiranno il postset della transizione di fine).
A questo punto basta completare la nostra rete di petri colleggando tutte le attività con posti intermedi rappresentanti la macchina tra un'attività e l'altra.

# Modelli delle attività ad un evento e due stati

![](https://i.imgur.com/oTNiMJU.png)

Semplifica il modello a due eventi e tre stati modellando l'intera attività con una transizione. Non permette di tener conto dello scorrere del tempo. Non adatto a modellare sistemi in cui è descritto cosa succede tra due eventi.

# Modelli a FMS
Sono modelli flessibili in cui le tipologie di prodotti possono essere cambiate. Presentano un elevato sfruttamento delle risorse, dunque il modello presenta un'utilità nell'analisi di deadlock o delle prestazioni. Non distinguono lo stato operativo da quello di attesa.
Presentano restrizioni su molti apetti tranne l'utilizzo di risorse.

