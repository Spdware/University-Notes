SDN Index: [[Software Defined Networking]]
Index: [[Index]]

---
Si basa sulle funzioni di gestione di rete. Informano il gestore della rete sullo stato della rete e permettono al gestore della rete di modificare alcune impostazioni di rete. Permettono anche di controllare il traffico passato per un certo nodo.
Presenti anche contatori su pacchetti sia in forma aggregata che disaggregata.
Sono usati molto assiduamente in macchine pensate per un uso pubblico.
I contatori sui vari pacchetti aumentano all'aumentare del luogo in cui vengono usati fino ad arrivare a milioni nei dispositivi usati dai distributori del traffico di rete.
Due obiettivi:
- misurarla per renderla osservabile. Nessuno sa come si comportano le reti, le loro dinamiche sono difficilmente prevedibili. Per conoscere quale rete sia migliore bisogna metterle in campo ed osservarne il comportamento/decidere dove è necessario intervenire.
- per ingegneria del traffico: strumenti atti alla modifica del traffico di rete.
Per sfruttare di più una rete dobbiamo modificarne la struttura. 
## Misurare il traffico
Vuol dire contare pacchetti e contare byte: contare quanti pacchetti/byte sono passati da una interfaccia. Poi posso fare disaggregazioni dividendo i pacchetti in base all'IP, alla tipologia di pacchetto(streaming, messaggistica, etc...)
La misura del traffico da input avare operazioni:
- identificare i flussi più significativi, individuare cosa usano di più gli utenti(si osserva la vita della rete). Usati per pianificare la capacità della rete/l'ammodernamento/potenziamento della rete.
- identificazione di attacchi: modifica del traffico della rete da situazioni standard.
- accounting e billing: attribuire l'uso delle risorse a chi le sta usando, tariffare il traffico di rete(generare le fatture)
- verifica dei livelli di servizio: misurano anche la quantità di traffico da gestire anche in base ad utenti business
Le misure del traffico da noi considerate sono passive, anche se esistono misurazioni di traffico attive(mandare pacchetti e misurarne la ricezione, il comune speed test).
## Traffic matrix

![|300](https://i.imgur.com/yvoLx46.png)

In questa rete si possono individuare collegamenti interni(non numerati) e collegamenti esterni(numerati)
La matrice di traffico è una tabelle con righe e colonne pari al numero di collegamento verso l'esterno e collegamenti verso l'interno. Sarà una matrice quadrata e nell'elemento $T_{ij}$ ci sarà il traffico totale di un elemento entrato tramite i ed uscito tramite j in un intervallo di tempo più o meno lungo(più è lungo più è mediato ed esatto, attenzione che il traffico di rete è stagionato(si basa sul periodo di traffico sulla rete)).
La misura oraria è una misura efficace su un periodo di tempo ideale.
I percorsi tra due punti di ingresso/uscita possono essere multipli. La misura del traffico comprende la somma dei percorsi, ma non permette di conoscere la percentuale percorsa lungo un determinato percorso, visto che potrebbe anche cambiare durante la misurazione.
L'interesse è solo calcolare quanti pacchetti entrano da un'uscita ed escono da un'altra.
Le misure sono dipendenti dal tempo, possiamo dividere in tante matrici in base alla tipologia di traffico. Possiamo misurare il traffico entrante/uscente dai vari ingressi/uscite ma non possiamo misurare il traffico entrante/uscente da una determinata coppia di entrata/uscita.

![|200](https://i.imgur.com/cm3EmOG.png)

Problema: misure aggregate sul bordo e vogliamo stimare cosa ci sia dentro, dobbiamo fare ipotesi. 
Vincoli sulla matrice:
- diagonale principale deve essere 0
- la somma delle righe e colonne è conosciuta
- la somma dei totali di riga deve essere uguale alla somma dei totali di colonna(rete di puro transito)
Sono presenti 2N+1 equazioni con $N^2 - N=N(N-1)$ incognite, dunque il sistema di equazioni è sottovincolato dunque ci sono infinite soluzioni. Bisogna fare ipotesi aggiuntive per introdurre altre equazioni. Da questo momento si introduzono congetture:
### Metodo gravitazionale
Metodo che aggiunge ipotesi non precise per poter introdurre equazioni
Si basa sulla congettura che il traffico entrante da un link si dispone sugli altri link in base al traffico di uscita dei nodi di uscita in modo proporzionale. Questo permette di aggiungere equazioni su $t_{ij}$ : 
$$T(i,j)=\frac{T^{in}T^{out}}{\sum_{j=1}^{N}(T^{out}(j))-T^{out}(i)}$$

![|300](https://i.imgur.com/CRqek0C.png)

Modello poco accurato visto che si discosta molto dal valore reale(linea continua).
Il modello potrebbe essere reso più preciso tenendo in considerazione tutti i percorsi interni e tenere conto del traffico interno(equazioni tomografiche). Questo porta ad un problema sopravincolato ed è necessaria una strategia per ridurre le grandezze.

## Exact counting
Come posso contare i pacchetti nei nodi?
Metodo facile in proncipio: uso un contatore e lo incremento ogni volta che vedo un pacchetto. Problema sorge quando devo aggiornare più contatori contemporaneamente e al fatto che necessito di una memoria abbastanza veloce e che sia capace di gestire anche milioni di accessi con anche numeri molto grandi e deve potre essere scritta anche più volte a pacchetto. Soluzione meno praticata poichè più costosa.
Posso mettere una memoria più economica usando qualche tipo di caching? Si e no visto che possono portare sia vantaggi che svantaggi dovuti ad inconsistenze. In questo caso non metto in cache gli ultimi contatori aggiornati ma devo sfruttare una gerarchia di memoria con memoria veloce ma piccola e memoria grande ma lenta. Trucco è mettere in cache i bitt aggiornati più di frequente(Ultimi bit del contatore, LSB) dunque tengo in considerazione due versioni dello stesso contatore. Aggiorno solo i contatori nella memoria veloce e periodicamente scelgo un contatore dalla memoria veloce, lo sommo al contatore nella memoria lenta e lo azzero(devo azzerare i contatori veloci prima che vadano in overflow). Devo scegliere il contatore da resettare nel minor tempo possibilie e dunque lìalgoritmo di scelta non deve dipendere dal numero di contatori. Per garantire il conteggio corretto è indispensabile che tutti i contatori siano scaricati prima che vadano in overflow. 
**Bit da affidare ai contatori veloci**

![|400](https://i.imgur.com/cbtCBuh.png)

- c: lunghezza dei contatori veloci
- M: lunghezza dei contatori lenti(M>c)
- N: numero dei contatori
- b: numero dei pacchetti dopo cui aggiorno un contatore veloce dumpandolo nella memoria lenta, è il fattore di velocità tra memoria lenta e memoria veloce 
All'arrivo di ogni pacchetto aggiorno un contatore veloce, ogni b pacchetti arrivati aggiorno un contatore lento con il corrispettivo contatore veloce che resetto. Cerco di resettare contatori con valore vicino a b. Soluzione naive è quella di aggiornare il contatore più grande(Largest Count Algorithm), dice che non c'è overflow se $c\approx \log\log(bN)$. Problema relativo al fatto che c'è un costo per trovare il contatore più lungo lineare con il numero di contatori(non va bene). Allora si passa al LCF semplificato che può essere eseguito più velocemente:
- S: array dei contatori nella memoria veloce
Ogni b pacchetti facciamo dinvetare j l'indice del contatore maggiornmente aggiornato nell'ultimo ciclo di pacchetti b. Se  $S[j]\ge b$  sommo e resetto S[j] altrimenti prendo un qualunque altro contatore che vale almeno b(lo cerco in una lista che mi tengo in memoria). Dobbiamo comunque aggiungere c ma sarà così piccolo che è trascurabile/rende l'algoritmo usabile.
$$c\approx\log$$

## Conteggio Randomizzato
Conto un pacchetto ogni tanto.
Quando un pacchetto arriva lancio una moneta truccata. Con probabilità p conto il pacchetto  con probabilità 1-p non lo conto. Dal punto di vista della misura esiste solo un sottoinsieme dei pacchetti. Campionamento di Possion. Chiamando c il reciproco di p mediamente aggiorniamo un contatore ogni c pacchetti(abbattiamo di un fattore c la scrittura in memoria). Visto la richieta di scrivere in memoria lenta non sempre il campionamento è totalmente randomico(Usare un campionamento deterministico porta a risultati peggiori, introduce dei bias nella stima). 
Ad un certo punto il contatore raggiunge il valore x, lo stimatore dei pacchetti arrivati mi dirà +/- il valore dei pacchetti arrivati realmente(conteggio esatto non è sempre richiesto, snobbabile se l'errore è accettabile e mi costa meno). Lo stimatore sarà xc(numero sbagliato). La precisione della stima dipende da n, numero dei pacchetti veri, se n è piccolo l'errore sarà grande mentre se n è grande l'errore sarà piccolo.
Funziona bene solo se usato per campionare flussi con un numero di pacchetti più grande rispetto a c, altrimenti sarà presente un rumore di misura.