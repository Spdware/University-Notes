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
