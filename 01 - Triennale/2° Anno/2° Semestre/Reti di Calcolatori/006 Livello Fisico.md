---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Fisico
cssclasses:
  - 
---

>Il **livello fisico** è la parte più bassa del [[005 Modello ISO-OSI|modello OSI]] ed è responsabile della *trasmissione fisica dei dati* attraverso il supporto di comunicazione, come cavi o onde elettromagnetiche.

Di seguito sono riportate delle definizioni riguardanti il livello fisico.

## Segnali
I **Segnali** sono variazioni nel tempo di grandezze fisiche che *trasportano informazioni*.

Le telecomunicazioni studiano la trasmissione di informazioni a distanza per mezzo di segnali che possono essere di vario tipo:
- acustico;
- **elettrico**;
- luminoso;
- elettromagnetico;
- ecc.

I segnali elettrici trasmessi da una linea possono essere di due tipi:
- *Analogici*: al variare del tempo, questi segnali possono assumere tutti i valori compresi fra i valori massimi e minimi consentiti dal canale di comunicazione. ^d0290f
- *Digitali*: al variare del tempo, questi segnali possono assumere solo due valori (o un numero discreto di valori). ^87f1d5

![[Immagine 2023-03-16 103253.png.png]]

Un segnale di periodo $T$ può essere sviluppato in [[Serie di Fourier]] in una somma di infinite sinusoidi.

Un segnale analogico è formato da *molteplici onde sinusoidali*. Ognuna di queste è caratterizzata da:
- **[[014 Funzione Periodica|Periodo]]** (Oscillazione): un'onda sinusoidale completa, quindi un intero ciclo di segnale;
- **Ampiezza**: indica l'altezza del picco dell'onda sinusoidale ed è proporzionale all'energia trasportata;
- **[[Serie di Fourier#^c10a2f|Frequenza]]**: è il numero di oscillazioni in un secondo, quindi il numero di periodi $T$, e si indica con $f=\frac{1}{T}$; ^76389c
- **Banda di Frequenza**: rappresenta l'intervallo di frequenze all'interno delle quali il segnale è attivo o in cui porta informazioni. È quindi il rapporto della grandezza delle informazioni da mandare sulla frequenza, ovvero $\frac{\text{grandezza\ info.}}{\text{frequenza}}$; ^b48258
- **Fase**: descrive la posizione dell'onda rispetto al tempo $0$. Indica la posizione del primo ciclo ed è misurata in gradi o radianti.

![[Pasted image 20230316114012.png]]

## Campionamento
>Il **Campionamento** è la conversione *da segnale analogico a segnale digitale*.

In generale, la frequenza di campionamento dovrà essere leggermente maggiore a $2B$, in modo da stabilire un piccolo range extra (*banda di guardia*) in modo da evitare che i filtri taglino parti utili del segnale.

## Rumore
Il **Rumore** è una forma di *energia indesiderata* che influisce sul segnale utile, degradandone l'informazione. È considerato, quindi, un *disturbo*.

## Teorema di Nyquist e Shannon (capacità del mezzo)
Shannon e Nyquist hanno enunciato teoremi che esprimono la massima velocità di trasmissione per ogni tipo di canale.

>Il **teorema di Nyquist** permette di stabilire la *massima quantità di informazione trasmessa (bit rate)* in un canale *non rumoroso*.

> [!quote] Teorema di Nyquist
> Dato un segnale con [[#^b48258|banda]] limitata $B$, si può ricostruire il segnale se la frequenza di [[#Campionamento|campionamento]] è maggiore o uguale a $2B$.
> Questa condizione garantisce che il segnale possa essere ricostruito senza perdita di informazioni.

>Invece, il **teorema di Shannon**, permette di stabilire il bit rate in un canale *rumoroso*.

> [!quote] Teorema di Shannon
> La frequenza di [[#Campionamento|campionamento]] deve essere almeno il doppio della [[#^76389c|frequenza]] massima presente nel segnale di ingresso, cioè la frequenza più elevata tra le sue componenti armoniche.

L'aumento dei livelli trasmissivi comporta l'aumento della quantità di informazioni che vengono trasmesse nello stesso tempo, ma il singolo livello diventa sempre più piccolo, rendendolo indistinguibile a causa del *rumore*, il quale è sempre presente.

![[Pasted image 20230316115649.png]]

## Trasmissione dei segnali
Una **Trasmissione di Segnali** è detta:
- *Analogica*: se il segnale viene trasmesso senza curarsi del suo significato. In tal caso la trasmissione si limita a recapitare il segnale, amplificandone l'intensità quando necessario.
- *Digitale*: se il segnale viene trasmesso tenendo conto del contenuto dei dati nel caso si debba amplificare l'intensità del segnale. Siccome l'amplificazione del segnale danneggerebbe il contenuto informativo, quest'ultimo viene estratto e si rigenera il segnale tramite opportuni ripetitori.

Una volta generato il segnale da trasmettere, questo può essere *immesso direttamente sul canale* (trasmissione in banda base), oppure si *immette su frequenze diverse* utilizzando la modulazione.

> [!info] La codifica dei dati
> I dati numerici vengono rappresentati dai segnali tramite *sequenze di impulsi discreti*. Il dato binario è quindi codificato in modo da far corrispondere il valore di un bit ad un certo livello del segnale.
> 
> Il ricevitore, cioè il dispositivo che riceverà i segnali, dovrà :
> - sapere quando inizia e quando finisce il bit; 
> - leggere il valore del segnale al momento giusto; 
> - determinare il valore del bit in base alla codifica utilizzata.
>
> La migliore valutazione si ottiene effettuando il campionamento del segnale al tempo pari a metà del bit.

^fbfa2b

## Modulazione
La **Modulazione** è un processo che permette di associare un segnale contenente delle informazioni (*segnale modulante*) ad un altro segnale (*segnale portante*) sviluppato ad alta frequenza.

Il risultato di tale processo è la *conversione del segnale modulante* dalla banda base alla banda traslata, generando quindi un nuovo segnale la cui banda sarà traslata.

Utilizzando una portante ad alta frequenza, si può quindi spostare la banda necessaria alla trasmissione in un range più opportuno.

> [!note]- Tecniche di Modulazione
>Esistono diverse tecniche di modulazione, alcune (le seguenti) riguardano l'ampiezza, la frequenza e la fase.
>- *Modulazione ASK (Amplitude Shift Keying)*: partendo da un segnale numerico, è possibile modulare in ampiezza un segnale portante moltiplicando la sua ampiezza per il segnale numerico. ![[Pasted image 20230316121619.png]]
>- *Modulazione FSK (Frequency Shift Keying)*: partendo da un segnale numerico, è possibile   modulare un segnale portante modificando la sua frequenza in funzione del segnale modulante, così facendo corrispondere le due frequenze a due valori del bit. ![[Pasted image 20230316121717.png]]
>- *Modulazione PSK (Phase Shift Keying)*: partendo da un segnale numerico , è possibile modulare in fase un segnale portante associando un certo valore di fase ad un certo valore di bit. ![[Pasted image 20230316121814.png]]

## Multiplazione
La **Multiplazione** è utilizzata per ottimizzare l'efficienza nella trasmissione di dati attraverso una rete condivisa. Specificamente, vengono *combinati più segnali* (analogici o digitali) *in un solo segnale* (detto *multiplato*) trasmesso in uscita su uno stesso collegamento fisico.

> [!note] Terminologie Multiplazione
> - Gli *slot* rappresentano unità di tempo o di frequenza all'interno del canale di trasmissione
> - I *frame* (trame) sono l'unità base del [[008 Livello Data Link|Livello Data Link]], ovvero dei pacchetti di dati.
> - Il *canale* si riferisce al mezzo fisico attraverso il quale avviene la trasmissione dei dati.

^bdfb6b

Ci sono diverse tecniche di multiplazione.

### Per segnali Digitali
#### TDM (Time Division Multiplexing)
Tecnica di multiplazione secondo la quale *ogni dispositivo ottiene a turno l'uso esclusivo del canale di comunicazione* per un breve lasso di tempo, quindi con la possibilità di sfruttare l'intera banda per sé. Esistono due metodi di multiplazione TDM.

##### Deterministico
Le trame vengono *allocate in modo fisso e nell'ordine stabilito*. 

![[Pasted image 20230316122616.png|500]]

> [!success] Vantaggi
> Il vantaggio è che le trame sono già ordinate e che non sono necessari schemi di indirizzamento.

> [!failure] Svantaggi
> Un grande svantaggio di questo è il caso nel quale non venga utilizzato il mezzo trasmissivo, infatti verrebbero mandate delle trame vuote, sprecando tempo e canale.

##### Statistico
Le trame vengono *allocate in modo dinamico*, in base alla quantità di dati da spedire. 

![[Pasted image 20230316122725.png|500]]

> [!success] Vantaggi
> Il vantaggio riguarda il fatto che le trame vangano allocate dinamicamente, evitando sprechi di tempo e canale.

> [!failure] Svantaggi
> Gli svantaggi riguardano la necessità di uno schema di indirizzamento e il fatto che sia più lento rispetto al TDM deterministico.

#### SDM (Space Division Multiplexing) 
Tecnica di multiplazione secondo la quale ad *ogni dispositivo viene associato un proprio canale*.  

![[Pasted image 20230316123136.png|600]]

<center>Differenza tra SDM (sopra) e TDM (sotto)</center>

#### CDM (Code Division Multiplexing)
Tecnica di multiplazione che moltiplica l'informazione binaria per una certa parola di codice detta *chip*. La sequenza in uscita dal moltiplicatore sarà successivamente modulata e trasmessa sul canale.

#### WDM (Wave Division Multiplexing)
Tecnica di multiplazione per segnali ottici molto simile alla FDM che consente di veicolare molteplici lunghezze d'onda all'interno dello stesso portante fisico.

### Per segnali Analogici
#### FDM (Frequency Division Multiplexing)
Tecnica di multiplazione secondo la quale *l'intero canale trasmissivo è diviso in sotto-canali*, ognuno costituito da una banda di frequenza e separato da un altro grazie ad un piccolo intervallo di guardia. Ciò rende possibile la condivisione dello stesso canale da parte di diversi dispositivi che utilizzano bande di frequenze diverse, in modo da poter comunicare contemporaneamente senza problemi di interferenza. In ricezione, opportune operazioni di demodulazione permetteranno di separare i diversi traffici. 

![[Pasted image 20230316123418.png|550]]

## CODIFICHE
Le codifiche servono per rappresentare segnali digitali in un segnale.

### RZ (Return Zero)
Lunghezza $T$ assegnata per ogni bit:
- Segnale nullo ($0$) per bit pari a 0; 
- Segnale di lunghezza $T/2$ su $V+$ per bit pari a 1.

![[Pasted image 20230922093218.png]]

### NRZ (Non Return Zero)
Uguale alla RZ, ma il bit pari a 1 può essere rappresentato sia su $V+$ che $V-$.

![[Pasted image 20230922093443.png]]

### AMI
Usa tre livelli sull'intero periodo $T$: 
- il livello 0 per il bit 0. 
- Il livello $V+$ oppure $V-$ per il bit 1.

![[Pasted image 20230922093607.png|400]]

### PSEUDO-TERNARIA
Uguale alla AMI ma con 0 ed 1 invertiti.

### MANCHESTER
È la codifica più importante. È composta da due livelli di tensione: 
- $V-$ per *la prima parte del periodo* per il bit 1; 
- $V+$ per la *seconda parte del periodo* per il bit 0.

![[Pasted image 20230922093851.png]]

### MANCHESTER DIFFERENZIALE
Usa lo stesso tipo del Manchester, ma rappresenta dopo ogni periodo $T$ si alternano le rappresentazioni (quello che prima rappresentava 0 ora rappresenta 1 e viceversa.

![[Pasted image 20230922094317.png|600]]

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```
Altri collegamenti: 
- [Andrea Minini](https://www.andreaminini.org/matematica/serie/serie-di-fourier)