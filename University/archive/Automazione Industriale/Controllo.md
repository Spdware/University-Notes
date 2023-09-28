Pagina Iniziale: [[Automazione Industriale]]
Indice: [[Index]]

---
# Sistemi retroazionati e controllore

![|400](https://i.imgur.com/m6h09jl.png)

![|400](https://i.imgur.com/s0O7wqq.png)

![|400](https://i.imgur.com/UiZ7AV9.png)

Per controllore si intende un insieme di posti che vanno a limitare lo scatto di alcune transizioni. Rappresentano dunque un vincolo da applicare all'impianto. Solitamente il controllore utilizza gli ingressi del processo per comporre la sua logica non considerando le marcature, mentre in uscita fornisce le variabili di controllo attraverso due sistemi:
- Associa le uscite all'inizio/fine di un evento(lega le uscite alle transizioni)
- Associa le uscite alle attività(lega le uscite ai posti). Dunque un'uscita rimane vera fintanto che l'azione è in corso

![|300](https://i.imgur.com/lPhB37K.png)

### Utilizzo di Reti di Petri per modellizzare impianto e controllore

![|300](https://i.imgur.com/y6THJED.png)

Si possono modellizzare sia impianto che controllore attraverso reti di petri senza modificare le operazioni di analisi.
Inoltre si può passare ad intendere le transizioni come l'accadere di un evento(reti sincronizzate). Si distingueranno così due situazioni:
- Marcature stabili: possono durare un tempo definito
- Marcature instabili: possono durare per un tempo infinitesimo
Nascono così anche due algoritmi di interpretazione:
- Approccio sincrono in cui si fanno scattare tutte le transizioni che possono scattare se un certo evento accade
- Approccio asincrono(controllo supervisivo): permette lo scatto di una sola transizione per ogni istante di tempo e per un dato evento. Permette così l'etichettature delle transizioni creando reti etichettate
### Progetto del controllore
#### Approccio diretto
Si parte a progettare il controllore dalle specifiche. Si analizzano le specifiche in modo da poter aggiungere le variabili di ingresso/uscita alla modellizzazione delle specifiche. Non presenta un processo di sintesi ma il controllore viene costruito a partire dai dati forniti. 

![|400](https://i.imgur.com/VV4PLrQ.png)

#### Approccio indiretto
Si progetta il controllore attraverso un processo di sintesi a partire dal modello del sistema da controllare. Si parte dai modelli delle specifiche e del sistema e si sintetizza un controllore che garantisca il corretto aderimento alle specifiche in anello chiuso e presenti anche caratteristiche notevoli.

#### Controllore e supervisore
La prima linea di pensiero è quella in cui il controllore si fa carico di definire le  
sequenze di lavorazione per i dispositivi e le macchine di un dato impianto. Il  
controllore decide quindi esattamente quando emettere quali comandi. Il  
comportamento specifico quindi di dispositivi e macchinari è dettato dal modello  
del controllore. 
Nella seconda linea di pensiero, invece, il controllore si preoccupa  
di intervenire sul sistema da controllare inibendo alcuni eventi, sulla base dello  
stato osservato. Un supervisore non decide quindi, in generale, l'esatta sequenza  
degli eventi. Per tale motivo, in questo caso si parla di "supervisore".
### Sintesi di un controllore
Utilizzeremo Reti di Petri per modellizzare sia il controllore che l'impianto. Utilizzeremo dei P-Invarianti per permettere al controllore di impedire il conseguimento di alcune marcature del sistema.

![|400](https://i.imgur.com/m4cDHSP.png)

Si vuole imporre sull'impianto il vincolo $$lM_p\le b$$ dove l è un vettore riga che contiene coefficienti interi, MP è la generica marcatura  
dell'impianto e b è uno scalare intero(Specifica del comportamento desiderato per il sistema in anello chiuso).
Possiamo supporre l'impedimento della marcatura di più posti contemporaneamente possiamo fare in modo che  $m_i+m_j\le 1$ (visto che i pesi apparterranno all'insieme {0,1} si parla di **mutua esclusione generalizzata**). Trasformiamo ora la disuguaglianza in un'uguaglianza aggiungendo una variabile ausiliaria non negativa e che rappresenta il posto del controllore: $$m_i+m_j+m_c=1$$
Tale equazione si può interpretare come la definizione di un nuovo P-Invariante della rete controllata, cioè della rete dell'impianto con l'aggiunta dei  posti di controllo. In pratica per ogni vincolo si introduce un posto nuovo  di controllo tale che, definendo opportunamente il peso degli archi che lo  collegano alle transizioni del processo, nasca un nuovo P-invariante della rete controllata.
### Sintesi del supervisore
Si supponga di avere una RP che modellizza l'impianto da controllare con matrice di incidenza $C_p$ · Siano n i posti e m le transizioni della rete  $C_p$ . Si supponga inoltre che su tale modello si vogliano imporre le seguenti specifiche di comportamento, scritte nella forma di disuguaglianza matriciale:
$$LM_p\le b$$
dove  $M_p$  è la generica marcatura del modello dell'impianto da controllare, L è una  
matrice di pesi interi di dimensione  $n_C x n$  e b è un vettore colonna di $n_C$   
componenti. Pertanto sono espresse  $n_C$  disuguaglianze lineari.
Si introducono $n_C$  variabili ausiliarie raggruppate in un unico vettore colonna  $M_C$ :
$$LM_p+M_C=b$$
n significato del vettore  $M_c$  è quello di marcatura degli ne posti di controllo che verranno aggiunti alla rete  $C_P$  al fine di imporre il soddisfacimento dei vincoli.
Sia  $C_C$  la matrice di incidenza che descrive la topologia del collegamento (cioè il  
peso degli archi) dei posti di controllo con le transizioni dell' impianto.
Descriviamo ora il sistema in anello chiuso:
$$C=\begin{bmatrix}  
   C_p \\  
   C_C  
\end{bmatrix}$$
Matrice di incidenza del sistema
$$M=\begin{bmatrix}  
   M_p \\  
    M_C  
\end{bmatrix},M_0=\begin{bmatrix}  
   M_{0p} \\  
   M_{0C}  
\end{bmatrix}$$
Marcatura generica e marcatura iniziale.
Abbiamo  $n_C$  P-invarianti raggruppati nella matrice X=[L I]' (ogni P-invariante è una colonna di X), tali per cui:
$$X'M=b$$
Essendo le colonne di X P-invarianti della rete complessiva (in anello chiuso), esse devono soddisfare l'equazione seguente:
$$X'C=\lbrack L\text{ }\text{ }I\rbrack \begin{bmatrix}  
   C_C \\  
   C_p  
\end{bmatrix}=0 \text{, cioè } LC_p + C_C=0$$
#### Sintesi del supervisore basata sugli invarianti
Se la marcatura iniziale dell'impianto soddisfa  $LM_{0p}\le b$   allora il supervisore a reti di petri con matrice di incidenza  $C_C$  e marcatura iniziale  $M_{0C}=b - LM_{0p}$  è in grado di imporre il vincolo $LM_p \le b$  quando inserito nell'anello di controllo.
Al fine .di comprendere $M_{0C}=b - LM_{0p}$ , si ricordi che per il calcolo della marcatura iniziale
del controllore da  $LM_p+M_C=b$  risulta che:  $LM_p + M_C= b =LM_{0p}+ M_{0C}$  .
Il metodo di sintesi qui visto è massimamente permissivo, cioè impone solo i vincoli richiesti, e non altri. Infatti, la condizione di abilitazione delle RP ci dice che lo scatto di una transizione è inibito solo se il suo scatto porterebbe a marcature negative. Pertanto, il supervisore inibisce transizioni che porterebbero a  $M_C < 0$ . Se tuttavia quest'ultima condizione fosse verificata, dalla  $LM_p +M_C= b$  si ricava che $LM_p>b$  .