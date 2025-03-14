Sono costituite da un grafo detto Bipartito poichè possiede due tipi di nodi:
- Posto: l'insieme dei posti si dice P={p1,p2,...,pn}
- Transizione: insieme delle transizioni si dice T={t1,t2,...,tn}
Relazioni che collegano gli stati sono detti **FLUSSO** e  $F\subseteq =P \times T \text{ }\cup\text{ }T\times P$
Definizione di Rete di Petri: RP=(P,T,F,M,W) con M marcatura(M:P $\rightarrow$ N) e  P Peso(P:F $\rightarrow$ N-0)
Conviene avere un operatore per riferirsi ai nodi a Monte/Valle(Preset/Postset)
PRESET(x)=${\{y\in P\times T|(y,x)\in F}\}=\dot{}$x
POSTSET(x)=x$\dot{}$=$\{y\in P\times T|(x,y)\in F\}$
# Regole di Evoluzione
- Transizione Abilitata: $\forall p\in \dot{}t,M(p)\geq W(p,t)$
- (Se abilitata) t può scattare:

|  |  |
|-----|------|
|M(p)=M'(p)|altrove|
|M'(p)=M(p) - W(p,t)|$p\in \dot{}t- t\text{ }\dot{}$|
|M'p)=M(p) + W(t,p)|$p\in t \text{ }\dot{}-\dot{}t$|
|M'(p)=M(p) - W(p,t) + W(t,p)|$p\in  \text{t } \dot{}\cap \dot{} \text{ t}$|

x- Può scattare una sola transizione alla volta(Eventi non accadono insieme)
- Scelgo la transizione da far scattare casualmente(Algoritmo Token Player)

![Imgur|350](https://i.imgur.com/Uqyniip.png)
![|400](https://i.imgur.com/q9E9lC3.png)

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
**Calcolo della totalità degli stati della Rete di Petri**
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
##### Calcolo della marcatura a partire dalle matrici di Input/Output
La nuova marcatura dovuta allo scatto di una transizione $t_j$ può essere calcolata partendo dalla marcatura attuale e andando a sommarci la colonna relativa alla transizione della matrice di output $O_j$ e a sottrarre la colonna relativa alla transizione della matrice di input $I_j$ :
$$\begin{cases}
M^*=M+O_j-I_j\\  
M\ge I_j    
\end{cases}$$
Ora possiamo notare che $O_j - I_j$ corrisponde alla colonna $C_j$ relativa alla transizione della matrice di incidenza:
$$\begin{cases}
M^*=M+C_j\\  
M\ge I_j    
\end{cases}$$
Per estrarre facilmente la colonna relativa alla transizione posso moltiplicare la matrice in considerazione con un vettore colonna della stessa dimensione delle righe della matrice ed avente 1 solo nella posizione della colonna che voglio ottenere. 
##### Equazione di stato delle Reti o Equazione fondamentale delle Reti
$$M^*=M+C s$$

Con s vettore colonna di m elementi(transizioni in considerazione, tutti $\ge$ 0) detto di Scatto o delle Occorrenze. Possiede la proprietà transitiva sullo svolgimento delle occorrenze, cioè non tiene conto dell'ordine dello scatto delle occorrenze. Quest'ultima proprietà comporta che il vettore s indica un insieme di sequenze fattibili ma non è garantita la loro correttezza(Non ne è assicurata l'esecuzione).
# Analisi formale della Rete
Sono presenti quattro strutture fondamentali legate alle caratteristiche statiche della rete:
- P-invarianti(Vettori colonna)
- T-invarianti(Vettori colonna)
- Trappole(Insiemi di posti) 
- Sifoni(Insiemi di posti)
Comportano un'analisi simile a quella fatta con le proprietà di Reversibilità, Limitatezza e Vivezza, ma meno approfondita

### P-Invarianti
Sono gli invarianti di tipo posto e corrispondono a insiemi di posti per cui la somma pesata dei gettoni contenuti rimane costante per tutte le marcature raggiungibili dalla rete. Definiti come vettore colonna di numeri interi con stessa dimensione del vettore marcatura e i cui coefficienti sono i pesi opportuni per la somma pesata(Devono essere positivi o nulli altrimenti non permetterebbero di limitare la rete).
$$\begin{cases}
x'M=x'M_0\\  
\forall M\in[M_0>   
\end{cases}$$
Corrispondono anche al trovare una combinazione lineare delle marcature di un insieme di posti per cui risulti un numero costante(Il P-invariante sarà l'insieme dei coefficienti della combinazione)
Sostituendo l'equazione di stato delle reti nella prima equazione possiamo ottenere che i P-invarianti possono essere trovati come soluzione di $x'C=0$ oppure $C'x=0$, $\forall s\ne 0$
I P-invarianti "simulano" la proprietà di limitatezza. Per limitare una rete mi basta trovare il P-invariante che copre tutti i posti della rete  oppure l'insieme dei P-invarianti che coprono l'intera rete. 
**P-invarianti particolari:**
- x'=\[0,....,0\]  è una soluzione valida però se sostituito in x'M=cost non permetterebbe di trovare soluzioni e dunque non è un P-invarante
- $x_A$ è un P-invariante e dunque $x_A'C=0$ , $2x_A$ è un P-invariante a sua volta($C'(2x_A)=2C'x_A=2*0=0$). Dunque una volta trovato un P-invariante ne ho trovati infiniti
- $x_A,x_B$  P-invarianti segue che un $x_C=x_A+x_B$ è un P-invariante e $C'x_C=C'(x_A+X_B)=C'x_A+C'x_B=0+0=0$
Dunque l'equazione c'x=0 può avere solo due soluzioni:
- 0 P-invarianti
- $\infty$ P-invarianti $\Rightarrow$ Voglio rappresentarne una lista finita
##### Supporto di un vettore
Insieme dei posti del P-invariante in cui l'elemento non è nullo, si indica con ||x||(Ex: ||x||={ $p_1,p_2,p_3$ })
##### P-invariante a supporto minimo
P-invariante il cui supporto non contiene quello di nessun altro P-invariante della rete
##### P-invariante canonico
P-invariante in cui il MCD dei suoi elementi non nulli è pari a 1
Ex: $x_A=[0 ,1,  0,  0]$
##### Generatore di P-invarianti
Più piccolo insieme di P-invarianti tale che ogni altro P-invariante della rete sia ottenibile da una combinazione lineare di questi P-invarianti. Gli elementi dell'insieme generatore sono detti P-invarianti minimi(Sono canonici o a supporto minimo). L'insieme generatore dei P-invarianti è finito e unico. 
##### Rete coperta da P-invarianti
Rete in cui tutti i posti appartengono al supporto di almeno un P-invariante. Per determinare facilmente se un insieme di P-invarianti copre rete basta calcolarsi il vettore somma dei P-invarianti e controllare che non ci siano elementi pari a zero.
Una rete coperta da P-invarianti è sempre limitata, ma non vale il viceversa, ovvero una rete limitata non è detto che sia anche conservativa
![|450](https://i.imgur.com/PzlZKed.png)

##### Rete conservativa
Rete coperta da soli P-invarianti non negativi:
$$\forall p\in P,\exists \text{P-invariante x | }p\in ||x||\text{ e }x(p)>0$$

##### Calcolo dei P-Invarianti minimi
Calcolati attraverso un algoritmo:
1. A:=C, Y:= In(Matrice identità di grado n), P:=[Y A] 
2. for i:=1 to m(numero transizioni della rete) do:
	1. Aggiungi a P tutte le righe che sono combinazioni lineari a coefficienti positivi di coppie di righe di P e che annullano la i-esima colonna della parte A della matrice P
	2. Elimina da P le righe in cui la colonna i-esima della parte A della matrice P è non nulla
 3. Le righe della parte Y della matrice P sono P-Invarianti positivi della rete. Tra essi sono presenti i P-Invarianti minimi

### T-Invarianti
Si definiscono T-invarianti di una rete N un vettore colonna y di dimensione |T| soluzione della seguente equazione: $Cy=0$
Dall'equazione di stato si deduce immediatamente che se y è un vettore delle occorrenze coincidente con un T-invariante allora: $M=M_0+Cy=M_0$
La presenza di un T-invariante non implica che sia davvero possibile ritornare alla marcatura iniziale poichè potrebbe non esistere alcuna sequenza di scatti ammissibile tale che il relativo vettore delle occorrenze sia proprio il nostro T-invariante. Un T-invariante implica solamente che se si fanno scattare le transizioni della rete un numero di volte pari a quello indicato in y allora la rete potrebbe tornare alla posizione iniziale. La proprietà di Reversibilità implica l'esistenza di uno o più T-invarianti.

### Sifoni
Un insieme di posti è un sifone se e solo se $^\circ S⊆ S^\circ$. Dalla definizione se ne deduce che tutte le transizioni di ingresso per un sifone sono anche transizioni di uscita.
Per un sifone possono esistere delle transizioni che sono solo in uscita e che quindi causano la perdita di gettoni. L'unione di sifoni è a sua volta un sifone.

![](https://i.imgur.com/amGU1dl.png)

**Sifone minimo**
Sifone S tale per cui non esistono altri sifoni S' tali che $S'\subset S$
**Sifone di base**
Sifone che non può essere ottenuto tramite l'unione di altri sifoni
**Sifoni non marcati**
Sifone privo di gettoni in una marcatura M e perciò privo di gettoni in ogni marcatura raggiungibile $M'\lbrack M>$
In presenza di un sifone smarcato la rete con S non è viva
**Calcolo dei sifoni**
Useremo un semplice algoritmo:
Si consideri una rete con matrice di ingresso I e matrice di uscita O
1. Si costruisce una rete con matrice di uscita O e matrice di ingresso I' nel seguente modo: per ogni transizione t della rete il peso degli archi entranti in t e uscenti dai posti $\in ^\circ t$ si calcola con: $I'(p,t)=k*I(p,t)$ con k somma dei pesi degli archi uscenti da t, definiamo $C^*$ la nuova matrice di incidenza
2. Si risolve la disequazione $x'C^*\leq 0$
3. Il supporto S=||x||, con $x\geq 0$ è un sifone della rete originale

### Trappole
Un insieme di posti S è una trappola se e solo se $S^\circ \subseteq ^\circ S$
Tutte le transizioni in uscita sono anche in ingresso.
Esistono transizioni solo in ingresso alla trappola che sono responsabili dell'aumento dei gettoni.
L'unione di trappole è a sua volta una trappola

![](https://i.imgur.com/iDnB70G.png)

**Trappole marcate**
Se S è una trappola marcata nella marcatura M essa rimane marcata in tutte le marcature $M'\in\lbrack M>$ 

### Sifoni, Trappole e P-Invarianti
Il supporto di un P-invariante con elementi non negativi è un sifone e una trappola.
### Sifoni, Trappole e Vivezza
Se ogni sifone contiene una trappola marcata in una marcatura M allora non esiste in $\lbrack M>$ (Insieme delle marcature raggiungibili) una marcatura morta. Se infatti esistesse una tale marcatura M' i posti senza gettoni costituirebbero un sifone marcato contraddicendo l'ipotesi.
Notiamo che non è assolutamente detto che una rete in cui ogni sifone contiene una trappola marcata sia viva.

