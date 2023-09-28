SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
Tutti questi algoritmi saranno visti in casi particolari ma possono essere usati anche in altre situazioni.
# Count-Min Sketches
Lo vedremo specializzato nel caso d'uso di identificazione di elefanti/heavy-hitter. Può essere usato per fare vari tipi di misure approssimate
Voglio capire chi sta usando più risorse, voglio vedere se ci sono nuove applicazioni,sorgenti, destinazioni che portano un volume più elevato di traffico.
Idea naive: arriva un pacchetto gli associo un identificativo numerico univoco in base alla tipologia del pacchetto e mi salvo in una tabella l'identificativo della classe e il numero di pacchetti usanti quella regola. Problema relativo alla memoria occupata dalla tabella(dipendente dal numero di classi rilevate, costo anche relativo all'aggiornamento della tabella). 
Provo allora un'altra strategia: 
1. parto dal pacchetto e identifico il flow ID(tipologia della categoria del pacchetto).
2. Calcolo l'hash della tipologia del pacchetto: h(f) con $h:{0,1}^*\rightarrow \{0,1\}^l \leftrightarrow [0,2^l-1]$ . L'array così ottenuto contiene il numero dei pacchetti aventi l'hash corrispondente all'indice della cella corrispondente. Faccio +1 se conto i pacchetti  $+l$  se conto i byte. Incremento la posizione i-esima e posso avere due casi: 
	1. Se il numero aggiornato è piccolo rispetto nua soglia il flusso non è un heavy-hitter
	2. Se il numero aggiornato supera una certa soglia allora il flusso f viene considerato un heavy-hitter(anche se dovrei cercare tra i flussi con hash h(f) ci siano delgi effettivi heavy-hitter). Per ridurre i falsi positivi posso ripetere la stessa operazione con diverse funzioni hash indipendenti in modo che lo stesso flusso f sia contato in diverse celle di memoria di diversi array con collisioni contro diversi flussi rispetto alle altre funzioni di hash. Se TUTTE le celle relative ad un flusso f sono considerate un heavy-hitter allora posso dire con una certa certezza che f sia un heavy-hitter. Se almeno in un array non supero la soglia di heavy-hitter allora non sono considerato un heavy-hitter
Il Count-Min Sketches usa una struttura dati a matrice con k righe(le funzioni di hash) e m colonne(m=  $2^l$  , l lunghezza dell'hash). Ogni volta che arrivo un pacchetto aggiorno la cella corrispondente.
Due primitive:
- Update(x,c): somma c al conteggio degli elementi di tipo x. Cerco in ogni riga la colonna a cui sommare c e lo sommo
- $\hat w$ = Estimate(x): stima il conteggio di x. Cerca il conteggio di x in tutte le righe e tengo il minimo valore
- N: invariante per riga, pari alla somma di tutti gli elementi in una riga
L'errore che può produrre si basa sul fatto che in media il conteggio medio sarà $\frac{N}{m}$ (Rumore di fondo dovuto al fatto che N si distribuisce uniformemente). 
$$Pr\{noise<\frac{2N}{m}\}=\frac{1}{2}$$
Allora cerceremo sempre la riga con rumore minimo(per cercare di mantenere il valore totale dovuto alle collisioni il minore possibile). $Pr\{\text{minimum noise}<\frac{2N}{m}\}=1-(\frac{1}{2})^k$ 
Dunque possiamo concludere che: $\hat w \le w+ \frac{2N}{m}$ con probabilità   $1-(\frac{1}{2})^k$ con w il conteggio corretto. Mi serve per resettare il tutto prima che il rumore di fondo arrivi alla soglia, altrimenti avrò tanti falsi positivi.

# Conteggio Probabilistico
Serve a contare quanti flussi unici sono state osservate oppure capire il numero delle applicazioni si sono connesse. 
Metodo naive consiste nel salvarsi l'elenco di tutte le connessioni che si sono osservate(non va bene per la memoria usata e per il problema di eliminare i doppioni).
Utilizziamo l'algoritmo di Flajolet-Martin la cui idea base è quella di rinunciare all'accuratezza, calcolo l'hash del flow ID e cerco pattern non comuni(maggiore è la probabilità di vedere pattern non comuni maggiore è il numero di flow ID diversi(L'evento inusuale è più facile trovarlo in una popolazione ampia)).
L'idea è che la funzione hash è una stringa di bit composta da 0 e 1. L'evento più probabile è che il primo 1 a sinistra sia molto a sinistra(Più andiamo a destra più è raro avere una stringa di soli 0). Nello specifico per una singola funzione hash la probabilità che abbia il primo uno nella posizione k è una distribuzione geometrica con probabilità 0.5(distribuzione discreta). $Pr[K<k]=1-1/2^k$
Noi estraiamo tanti numeri, uno per ogni flusso unico, dunque ci chiediamo quale sia la sequenza con più zeri a sinistra e ci chiediamo la probabilità che l'uno in quel flusso sia in posizione x con w flussi : $Pr[X<x]=Pr[K<k]^w= [1-1/2^x]^w$
Pr[X<x] è massima intorno a $x=\log_2(w)$. 
Maggiore è il numero di flussi maggiore sarà la probabilità che il primo uno da sinistra sarà spostato molto verso destra.
Possiamo creare un algoritmo:
1. Facciamo l'hash dei vari flow ID dei flussi che mi arrivano
2. In ogni hash trovo il primo 1
3. Tengo la posizione dell'1 più a destra(x)
4. Calcolo il mio stimatore: $\hat w= 2^x*\varphi, \text{ con }\varphi=0.77351$ 
La varianza dello stimatore è molto alta visto che posso sbagliare in modo molto ampio  miei calcoli causato dall'esponente. Per ridurre questa varianza faccio la media degli estimatori derivati dall'uso di diverse funzioni hash.
$A=\frac{1}{m}\sum_{i=1}^m x_i$, se consideriamo m in ordine di decine di funzioni hash ci permette di avere una buona stima con un ridotto consumo. 
# Hash Based Sampling
Campionamento basato su hash.
Cerca di risolvere il problema relativo al salvataggio di una parte dell'intestazione del pacchetto, cerca di far capire la strada che intraprendono i pacchetti nella rete.
Si cerca di osservare il percorso dei pacchetti all'interno della rete(lavoro non semplice visto che si salva la copia di un pacchetto in ogni nodo e poi vedere in quali altri nodi è passato).
Quasi impossibile salvarsi tutti i pacchetti(Memoria dovrebbe essere MOLTO veloce e una capacità di disco ENORME). Strada non percorribile. Si può usare il campionamento salvando una percentuale di pacchetti(problema relativo al fatto che se ogni nodo prende decisioni indipendenti le informazioni cambiano da nodo a nodo, misura imprecisa approccio diventa inutile). Dobbiamo prendere decisioni consistenti nella rete(se un pacchetto è campionato deve essere campionato in tutti i nodi che attraversa). Possiamo forzare questo comportamento prendendo una decisione che sembri casuale ma che dipenda dal pacchetto(posso aggiungere un'informazione al pacchetto, anche se è un'operazione costosa visto che devo modificare il pacchetto e ne allunga la dimensione(problema sui pacchetti di dimensione massima)). Altrimenti devo far dipendere la decisione dal pacchetto stesso(dal contenuto del pacchetto deve essere scelto se campionare o meno il pacchetto).
#### Strategia 1
Estriamo dal pacchetto alcuni campi(evito campi che cambiano lungo il percorso, ex type univ). A seconda della rete/configurazione devo scegliere campi che restano identici lungo tutta la vita del pacchetto. Scelgo poi una funzione hash unica per la rete che prende in entrata la concatenazione dei campi scelti e da in uscita un intero da 0 a r(potenza di 2, grande a scelta). Controllo poi se R è maggiore di una soglia B scelta, nel caso lo sia campiono altrimenti non campiono. Se la funzione di hash è scelta bene l'uscita può essere assoggettata ad un avariabile uniforme B/R. Così possiamo campionare intere traiettorie.
Campi non usabili: Type Of Service, Time TO Live, Checksum, Options

Sul disco salveremo un hash del pacchetto in modo che possa usare quello per tracciare il percorso del pacchetto nella rete(Permette anche di risparmiare sui dati scritti in memoria).
La funzione hash usata per campionare deve essere diversa dalla funzione hash usata per salvare l'impronta digitale del pacchetto(Dopo la campionatura non abbiamo più una distribuzione uniforme dei pacchetti). Anche questa funzione hash deve essere uniforme per tutta la rete.

# Bloom Filter
Supponondo di voler sapere la strada percorsa da un pacchetto all'interno della rete relativo ad un tentativo di intrusione(indirizzi alterati quindi inutile chiedersi la strada originale). Vogliamo tracciare l'interfaccia di entrata. Risaliamo al percorso all'interno della rete(non possiamo usare lo strumento precedente poichè traccia un flusso corretto di pacchetti, in questo caso i pacchetti sono solo una parte dei pacchetti in entrata e non compongono un flusso). Devo poter tenere traccia di tutti i pacchetti e non solo una percentuale. Vorrei avere in ogni nodo una struttura dati che risponda alla domanda se un pacchetto P sia passato in un nodo, poi nel caso interrogo i router a monte se il pacchetto è passato o meno. 
In questo caso voglio avere risposta per TUTTI i pacchetti(più efficiente di salvare al lista di pacchetti), stando cercando uno specifico pacchetto ho tutto il pacchetto per interrogare i nodi. Possiamo accettare un algoritmo che non dia sempre risultati corretti(Cerchiamo un algoritmo più efficiente di salvare l'hash di tutti i pacchetti, T(n)= cost, S(n)=cost e accettiamo che sia possibile avere risposte sbagliate).
Utilizziamo uno strumento chiamato FIltro di Bloom, algoritmo che risponde alla domanda se un certo elemento faccia parte di un certo indieme. Primitive: Inserimento e Test di Appartenenza(Non posso rimuovere elementi). Costituito da un array binario di lunghezza m=  $2^b$ (potenza di due). Inizialmente l'array è completamente riempito di 0. Presente una batteria di K funzioni di hash indipendenti che prendono una stringa di lunghezza arbitraria e danno in output una stringa di lunghezza b.(ex: h(x), $h_1(x)=h(1||x), h_2(x)=h(2||x)...$)
Devo estrarre dal pacchetto i campi che si mantengono inalterati, tramite essi ne calcolo hash tramite le varie funzioni di hash, localizzo l'indice corrispondente e lo porto a 1(operazione fatta per tutte le K funzioni).
Differenze con Count Min Sketch:
- Tutte le funzioni di hash agiscono sullo stesso array
- L'array è binario, dunque non si incrementa nulla oltre il passaggio da 0 a 1
- Più arivano pacchetti più il foltro diventa pieno(il numero di bit posti a uno determina il riempimento del filtro stesso)
Test di appartenenza si ottiene recuperando il pacchetto, facendo i K hash e controllo gli elementi corrispondenti, se trovo uno 0 allora il pacchetto non è passato dal nodo. Se trovo tutti 1 o l'elemento fa parte dell'insieme oppure ci sono state collisioni che mi fanno sbagliare(Risposta potrebbe essere sbagliata, Falsi Positivi).
Il rateo di falso positivo è determinabile ed è possibile tenerla bassa.
Dati richiesti:
- Lunghezza della funzione hash: b
- Lunghezza dell'array:  $m=2^b$
- Numero di funzioni hash: k
- Numero di elementi inseriti: n
Più inseriamo elementi più avrò opportunità di collisioni.
Probabilità che dopo n inserzioni un bin sia a 0:
$$p=(1-\frac{1}{m})^{kn}=exp(-\frac{kn}{m})$$
Probabilità di falso positivo(trovo tutti uno nelle posizioni di un nuovo elemento):$$(1-p)^k=(1-exp(-\frac{kn}{m}))^k$$
Se m cresce la probabilità di falso positivo diminuisce. 
Più n cresce più la probabilità di collisione cresce.
Se k è piccolo avrò una maggiore probabilità di falso posititvo, se k è grande il filtro si riempie molto più velocemente. Devo quindi prendere un valore ottimo di k minimizzante il numero di falsi positivi. K ottimo: k= m/n log 2. Restituisce una probabilità di falso positivo di $(0,62)^\frac{m}{n}$ . Per avere un tasso di falsi positivi di 1% devo avere m/n = 9,6 bit per ogni pacchetto che voglio inserire e devo avere k = 7 funzioni di hash.
Controllo su URL viene fatto localmente e si verifica se l'URL fa parte di quelle segnalate. Naturalmente non posso scaricare sui browser locali i listoni degli URL allora si pone il listone in un filtro di Bloom e lo si spedisce al client. Se trovo un URL falso posititvo mando un riscontro ai server google che mi risponderanno per indicare se è realmente malevolo.