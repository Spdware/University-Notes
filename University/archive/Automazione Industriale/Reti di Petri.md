Sono costituite da un grafo detto Bipartito poichè possiede due tipi di nodi:
- Posto: l'insieme dei posti si dice P={p1,p2,...,pn}
- Transizione: insieme delle transizioni si dice T={t1,t2,...,tn}
Relazioni che collegano gli stati sono detti **FLUSSO** e $F\subseteq =P \times T \text{ }\cup\text{ }T\times P$
Definizione di Rete di Petri: RP=(P,T,F,M,W) con M marcatura(M:P$\rightarrow$N) e  P Peso(P:F$\rightarrow$N-0)
Conviene avere un operatore per riferirsi ai nodi a Monte/Valle(Preset/Postset)
PRESET(x)=${\{y\in P\times T|(y,x)\in F}\}=\dot{}$x
POSTSET(x)=x$\dot{}$=$\{y\in P\times T|(x,y)\in F\}$
# Regole di Evoluzione
- Transizione Abilitata: $\forall p\in \dot{}t,M(p)\geq W(p,t)$
- (Se abilitata) t può scattare:
| | |
|-----|------|
|M(p)=M'(p)|altrove|
|M'(p)=M(p) - W(p,t)|$p\in \dot{}t- t\text{ }\dot{}$|
|M'p)=M(p) + W(t,p)|$p\in t \text{ }\dot{}-\dot{}t$|
|M'(p)=M(p) - W(p,t) + W(t,p)|$p\in  \text{t } \dot{}\cap \dot{} \text{ t}$|
- Può scattare una sola transizione alla volta(Eventi non accadono insieme)
- Scelgo la transizione da far scattare casualmente(Algoritmo Token Player)

![Imgur|400](https://i.imgur.com/Uqyniip.png)
![Imgur](https://i.imgur.com/q9E9lC3.png)

# Come modellare un sistema da una Rete di Petri
L'insieme dei posti/gettoni nei posti si associa al concetto di stato(Distribuzioni dei token in uno stato)
Le transizioni rappresentano la transizione tra stati
### Passi da seguire per lo svolgimento dell'operazione
1. Esamino il problema e lo divido nelle varie componenti
2. Esamino ogni singola componente e cerco quali e quanti stati inserire
3. Semantica/Vocabolario: dare significato al sistema nominando ogni stato/transizione. La definizione di posti(Aggettivi)/transizioni(Verbi/Azioni) vincola la definizione di transizioni/posti
4. Devo controllare che i singoli elementi siano collegati correttamente. I pesi degli archi sono usati per modellare il numero di token estratti
# Proprietà Comportamentali
Notazioni:
- $\text{ [ M >}$ :Insieme di Raggiungibilità(Parti dalla maggiore e vai avanti)
- $\text{M [ } t_1 > M_2 ...$ : $M_2$ raggiungibile da M attraverso lo scatto della transizione $t_1$
- $M\in [ M>$ : M appartiene all'insieme di Marcature M >
Proprietà:
- Reversibilità : se da un qualunque posto tu possa tornare allo stato iniziale 
		$\forall M\in[M_0>,M_0\in[M>$
- Limitatezza: ogni posto della rete deve contenere un numero limitato di gettoni
	- p è k-limitato $\iff$ $\forall M\in[M_0>,M(p)\in k$
	- p è limitato $\iff$ è k-limitato per qualche k
	- Una rete è k-limitata $\iff$ $\forall p\in P$ è k-limitata
	- Una rete è limitata se è k-limitata per qualche k
- Vivezza: t è detta viva $\iff$ $\forall M\in[M_0\ge\exists M^*\in[M>:M^*[t>$  
	- Una rete è viva $\iff$ t è viva, $\forall t\in T$ 
	- Tutte le transizioni di una rete viva possono scattare infinite volte
![Imgur|300](https://i.imgur.com/dzekwwr.png)
##### Commenti sulla Vivezza
Definisce una transizione che può scattare infinite volte
Una transizione morta scatta 0 volte e può definire una non esecuzione
Una marcatura è viva se posso trovare strade per cui posso far scattare tutte le transizioni della rete(Non compresa nella Vivezza)
##### Marcatura Morta
Marcatura che non può abilitare nessuna transizione(Lo stesso si applica al concetto di transizione morta: transizione che non può raggiungere nessun posto)
### Metodi Investigativi per la Ricerca delle Proprietà
##### Calcolo della totalità degli stati della Rete di Petri
- Calcolo il grafo di raggiungibilità(Sostituibile con l'albero di raggiungibilità)
- Rappresento la totalità delle marcature
Una marcatura può essere rappresentata come un vettore colonna in cui 1 indica la presenza di un token nel posto corrispondente
Il disegno del grafo è simile ad un automa a stati finiti in cui lo stato rappresenta la marcatura e sulla transizione viene posta la transizione che scatta per arrivare allo stato successivo
Come dedurre le proprietà:
- Reversibile: il grafo deve tornare nello stato iniziale
- Limitata: la rete deve essere rappresentata da un numero limitato di scatti/stati
- Vivezza: devono essere presenti tutte le transizioni e devono essere tutte percorse
**Attenzione**: la vivezza non può essere dedotta nel caso in cui non si conosca la Rete di Petri(in quel caso bisogna far supposizioni sulle transizioni presenti)
##### Studio delle proprietà formali(metodo algebrico)
Convenzioni:
- Il grafo è una serie di nodi collegati tra loro. Essendo i nodi di due tipi sono richieste due matrici definite Matrici di Ingresso/Uscita(I/O) in cui le righe rappresentano i posti mentre le colonne le transizioni e il contenuto della cella rappresenta il peso dell'arco(=0 se non esiste il flusso tra posto e transizione)
- Matrice di Input tiene conto dei flussi che collegano un posto a una transizione

![|100](https://i.imgur.com/JrNZUqD.png)
- Matrice di Output tiene conto dei flussi che collegano una transizione a un posto

![|100](https://i.imgur.com/FugVRoV.png)

**Matrice di Incidenza**
Per comodità si può usare una matrice detta di Incidenza in cui le celle contengono la differenza tra le celle della matrice di Output e di quella di Input(C = O - I)
La matrice di Incidenza non è equivalente alle altre due poichè la differenza dei flussi potrebbe essere 0(Auto-anello) portando così a una scorretta abilitazione delle transizioni. Per questo è utile solo in presenza di Reti di Petri Pure(Senza auto-anelli) altrimenti sono più utili le matrici di Input/Output
Un calcolo alternativo e più sicuro per il calcolo della matrice di Incidenza consiste nell'immaginare di far scattare le transizioni una ad una e immaginarsi gli effetti netti sulla marcatura corrente:
1. Tolgo i gettoni a monte della transizione
2. Aggiungo i gettoni a valle della transizione
3. Ignoro i posti non raggiungibili
Questo metodo è utilizzabile sia per le Reti di Petri Pure che non anche se per quest'ultime non ha molto senso
