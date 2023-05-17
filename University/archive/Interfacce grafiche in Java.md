Se rmi non si avvia creare classpath nel server
# Swing
Vecchio stile delle interfacce grafiche. Da una maggiore flessibilità sulla gestione degli errori parte grafica.
## Cosa serve per creare una interfaccia grafica
Le GUI usano un paradigma ad eventi, non è possibile gestire l'applicazione tramite eventi. Gli elementi sfruttano il concetto di evento per passarsi messaggi.
Due classi di componenti:
- Generatori: inviano messaggi
- Listener: ricevono messaggi dai generatori
Nessuno dei due conosce l'altro, il loro unico interesse è il messaggio.
Si passa a un concetto di programmazione reattiva.
Non è possibile definire un unico flusso di controllo ma vanno definiti diversi flussi di controllo sincronizzati da messaggi.
Vantaggi: 
- Si basa sulla possibilità di aggiungere/togliere componenti a runtime. Paradigma publish/subscribe(Pattern diverso da observer/observable poichè presenta un componente centrale detto event-broker che distribuisce i vari messaggi, permette la non sottoscrizione a svariati oggetti per dover comprendere la previsione di diverse situazioni, observer ha senso se i componenti sono pochi e dedicati)
Per applicazioni event driven ogni interazione deve essere specificata all'interno di un evento con il suo contesto(Concetto di evento contiene il tipo di evento e il contesto).
Swing è la prima libreria che generalizza ampiamente le GUI rendendole praticamente platform independent(Non tutte le piattaforme devono gestire determinate situazioni).
Rispetto ad AWT è più lenta(Non usare se bisogna gestire animazioni complesse).
Swing è fatto a componenti. Tutte le classi dei componenti iniziano con la J, sono detti componenti leggeri e sono definiti in java(Poi si appoggia a AWT per visualizzare a schermo).
Compatibile con MVC, Presenta una logica, una parte visiva e una parte di modello che viene aggiornato dalla GUI.
## Elementi
- Container
- Elementi Grafici
- Eventi
## Gerarchia
- Component: classe generale che fornisce metodi di base per ogni elemento di una interfaccia grafica. Ogni component dovrà fornire una visualizzazione a schermo. 
- Container: Tipo specifico di componente, può contenere altri componenti e permette di fornire una rappresentazione ad albero/permette di fornire implementazioni gerarchiche ad albero. Permette di decidere il livello a cui porsi per guardare gli eventi. Presenta metodi come add, remove. Due tipologie:
    - Top level: non devono essere per forza all'interno di un container per visualizzare qualcosa. NEcessario averne almeno uno in una gui
    - Common Layer: contenitori che possno risiedere in qualsiasi livello
  Il modello dei container è gestito a livello. 
    - root pane gestisce gli altri pannelli
    - layer pane: gestisce i vari livelli
    - content pane: pannello all'interno del layer pane in cui deve essere messo il contenuto del pannello. Se serve una menù bar andrà definita tramite API e poi gestita da librerie Swing
    - glass pane: serve solo per catturare eventi dal OS
- JComponent: componente qualsiasi, superclasse di qualsiasi component. Contiene il concetto di Window che è un container concesso dal sistema operativo per disegnare qualcosa
- Frame: definisce la struttura dell'interfaccia
- JFrame: prima implementazione che fornisce un root pane, posso disegnarci qualcosa, posso definirne la dimensione, settare la posizione rispetto angolo sinistro finestra/schermo. Devo ricordarmi di rendere visibile una finestra. Posso settare anche cosa succede se chiudo l'intera finestra. Non posso fare il throw diretto degli elementi, devo aggiungere al frame altri elementi. frame.pack() è il metodo per permettere di prendere tutto lo spazio richiesto/permette di occupare lo spazio efficientementemente.
- JDialog: può specificare messaggi all'interno, gestisce le notifiche tra finestre. Serve per non implementare a mano alcune tipologie di frame. Permette anche di far sentire effetti sonori, permette anche di settare testi specifici per i punsanti e farlo diventare di default. Va in primo piano e obbliga la sua gestione(Maniera sincrona).
- JPanel: permette di gestire gerarchie
- JScrollpane: serve per implementare un'area di testo che sia possibile scorrere
- JSplitPane: permette di gestire due pannelli divisi da una barra
- JTabbedPane: permette di gestire diversi pannelli come tab
- JInternalPane: permette di gestire finestre simulate nello spaxio
- JLayeredPane: permette di gestire più pane nel pannello. Sono predefiniti(Guardare slide)
- JToolBar: permette di strutturare una toolbar diversa dalla menù bar, permette di gestire azioni semplici
- JButton: bottoni, attenzione che possono essere create combo di bottoni
- JComboBox: menù a tendina
- JList: crea una lista
- JMen: permette di gestire un menù in maniera semplice
- JTextField: permette di inserire testo
- Jlabel: permettono di mostrare testo/immagini
- JToolTip: esposto come proprietà di un componente
- JProgressBar: barre di progresso, gestibile a mano
- JColorChooser: selezione del colore
- JTree: permette di implementare un file esplorer
- JTextComponent: generico testo
- JTable: permette di visualizzare una tabella senza fare l'allineamento a mano
### Look and Feel
Permette di decidere lo stile dei frame.
### Layout
Sono specificati dai container. Sono incapsulati in una classe. L'istanza della classe va passata al metodo di set del container. Se lo cambiamo mentre qualcosa è visibile va chiamata la clear sul container. C'è nè uno solo per container.
- FlowLayout: permette di mettere tutto uno di fila all'altro, può essere allargata alla finestra per mostrare più contenuto
- BorderLayout: utile perchè definisce 5 posizioni. Nord e Sud espanse verticalmente, Ovest e Est espanse orizzontalmente, Centre espanso su tutte le direzioni.
- GridLayout: specifica una griglia. Si specifica prima sulle righe e poi sulle colonne
- GridBadLayout: permette una griglia ma più potente e più complessa
### Borders
I bordi non sono inseriti su tutti i componenti. Per inserirli si fa come per i layout tramite una factory specifica.
## Eventi
Per ogni evento generato avremo la source, l'oggetto e 
- EventListener: interfaccia generale che permette idi invocare un metodo simile ad update. Ogni listener va associato ad un elemento
**Come implementarli**
- DIchiarazione
- Associazione
- Definizione
Tipologie di eventi:
- Pulsanti schiacciati
- Mouse event: tasto destro, sinistro, rotellina, spostamento del mouse
- Tastiera premuta
- ...........
Dunque sull'evento oltre la source posso chiedere modificatori e comandi, posso sapere come è stato generato, dove è stato generato.
Se non sono interessato ad un particolare evento lo dico esplicitamente lasciando vuoto il metodo corrispondente.
### Swing thread
- Thread iniziale: thread da cui viene generata la gui
- Event Dispatch thread: resta in ascolto degli eventi e li distribuisce agli interessati. Solitamente è presente una coda di eventi arrivanti all'event-dispatcher che li girerà al listener corrispondente. Serve una coda per mantenere un minimo di continuità temporale. Possiamo metterci mano solo tramite API specifiche. Gli eventi sono messi in coda tramite il comando later.
- Worker thread: sono utilizzati per gestire qualcosa che richiede del tempo, permette di non bloccare l'interfaccia grafica. Possibile usare SwingWorker. Permettono di ridare il flusso di controllo generale alla gui principale.
# JavaFX
Metodo per implemetare la struttura della gui tramite FXML(simil HTML). Utilizza un markup language. Utile usare SIMBUILDER.
Possibile convertire da Swing a JavaFX.
Utilizzare soltanto se si vuole fare una grafica più carica.
Tolta la parte relativa al property binding non è diverso da Swing.
# Nozioni nate da Esempi/Esercizi
setPreferredSize() richiede di settare le dimensioni della finestra tramite un oggetto non due parametri.
Per creare una gui devo seguire i segueti passaggi: Frame->Layout->Button e a ritroso Layout.add(Button)->Frame.add(Layout)
RIcordarsi sempre di rendere visisbile il frame.
Ricordarsi di collegare gli eventi.
Per collegare le azioni è utile collegare il tutto tramite classi anonime e lambda.
Se devo conoscere un'area di dove si trova un pulsante è utile crearsi un pulsante specifico.
Potrebbe capitare di dover usare il pattern producer/consumer.
Utile introdurre in una lambda dei comandi che voglio passare da un thread in background al thread della GUI. Worker thread sono utili anche per i timeout.
Sempre utile avere a disposizione le labels per comunicazione. Stare attenti anche all'ordine di definizione delle labels.