Pagina Iniziale: [[Automazione Industriale]]
Indice: [[Index]]

---

Sono apparecchiature elettroniche programmabili per il controllo di macchine e processi industriali
Sostituirono la logica cablata e i quadri di controllo a relè(riduzione di tempi e costi)
Resistono agli ambienti industriali, non presentano dischi mobili, sono dotati di Watch-Dog(per istruzioni/programmi), SO molto affidabile e ad elevata diagnostica sia su HW che SW, compatto, integrabile con altri componenti di controllo

### Ciclo del PLC
Essendo un dispositivo a segnali campionati il PLC durante il funzionamento esegue ciclicamente tre fasi fondamentali:
- Lettura ingressi: legge i segnali provenienti dai moduli di input e  li salva in RAM(**Non posso mai scrivere sugli ingressi**)
- Esecuzione del programma: esegue il programma presente in memoria usando i dati in input e i dati presenti in memoria
- Scrittura uscite: setto i segnali da mandare ai moduli di output con i valori calcolati durante l'esecuzione del programma(Vengono settate una sola volta per ciclo non ad ogni loro ricalcolo nel programma, posso sia leggere che scriverci sopra)

![[Pasted image 20230315104303.png]]
![[Pasted image 20230315104336.png]]

Questo funzionamento viene detto a **Copia Massiva**(Scrittura e lettura di tutte le uscite e tutti gli ingressi avvengono contemporaneamente)
VANTAGGI:
- Semplice da implementare e da capire
- Semplice da simulare
SVANTAGGI:
- Scarsa reattività
- Scarso sfruttamento delle risorse
Per ovviare agli svantaggi alcuni PLC hanno cicli di funzionamento diversi in cui eseguono la lettura e scrittura di ingressi e uscite anche mentre stanno eseguendo il programma aumentando la frequenza di aggiornamento , oppure usano **interrupt**

###### Interrupt
1. Interrompono il ciclo di programmazione
2. Eseguono una opportuna subroutine di gestione dell'interrupt
3. Restituiscono il controllo al programma
Esistono sia di tipo HW(Generati da una risorsa fisica esterna, garantiscono una risposta rapida ad un evento) sia di tipo SW(Generati periodicamente ad intervalli regolari, usati per campionare ingressi in tempi inferiori al tempo di ciclo)

#### Modalità operative
*Modalità di esecuzione*
Il PLC funziona come controllore: legge gli ingressi, esegue il programma di controllo e aggiorna le uscite
*Modalità di validazione*
Il PLC esegue il programma ma non legge ingressi fisici e non scrive su uscite fisiche: usa variabili intermedie eventualmente connesse con il sistema di programmazione
*Modalità di programmazione*
Il PLC è connesso con il sistema di programmazione e accetta scritture della memoria programmi

#### Watch Dog
Sono sostanzialmente timer, che il sistema operativo associa a svariati componenti del PLC e all’esecuzione di svariate operazioni
Ad un'ioperazione è associata una durata massima e se il timer raggiunge quell'intervallo di tempo prima che l'operazione sia conclusa genera un errore

### PLC da normativa IEC61131
Sistema elettronico a funzionamento digitale, destinato all’uso in ambito industriale, che utilizza una memoria programmabile per l’archiviazione interna delle istruzioni orientate all’utilizzatore per l’implementazione di  funzioni specifiche, come quelle logiche, di sequenziamento, di  temporizzazione, di conteggio e di calcolo aritmetico, e per controllare,  mediante ingressi ed uscite sia digitali che analogiche, vari tipi di macchine e processi.
![[Pasted image 20230315110724.png]]
- Configuration: corrisponde as un ProgrammableControllerSystem, cioè generalmente ad un PLC
- Resource: costituiscono il supporto di esecuzione dei programmi. Le Resource sono autonome tra loro
- Task: specifica l'attivazione di parti di programmi o interi programmi loro assegnati(Cyclic/EventDriven Task)
- Program: sono eseguiti sotto il controllo di zero o più task. I Program sono dei contenitori di costrutti eseguibili, scritti nei linguaggi previsti dallo standard
##### Funzioni
Sono porzioni di costrutto eseguibili che restituiscono un valore dipendente dagli ingressi senza avere variabili "interne"(di stato)
Sono definibili dei blocchi funzione (routine di codice, dotate di variabili interne o di stato), possono essere salvati in librerie e riutilizzati in vari progetti indipendentemente dal linguaggio utilizzato
###### Tipi di dato
Variabili globali e locali dichiarate in una ProgramOrganizationUnit con nomi mnemonici
Tipologie(B = byte, b = bit):
- Bit Strings - groups of on/off values (BOOL(1), BYTE(8), WORD(16), DWORD(32), LWORD(64))
- INTEGER - whole numbers (SINT(1B), INT(2B), DINT(4B), LINT(8B))  
- U - Unsigned - add a U to the type to make it unsigned integer  
- REAL - floating point IEC 559 (IEEE) (REAL(4B), LREAL(8B))  
- TIME - duration for timers, processes.  
- Date and Time of day (DATE, TIME_OF_DAY, DATE_AND_TIME)  
- STRING - character strings surrounded by single quotes
- WSTRING - holds multi-byte strings  
- ARRAY - multiple values stored in the same variable  
- Derived - type derived from one of the above types  
- STRUCT - composite of several variables and types.  
- Generic (ANY)
Le variabili che devono essere persistenti al *riavvio a caldo* possiedono la proprietà di retain 

