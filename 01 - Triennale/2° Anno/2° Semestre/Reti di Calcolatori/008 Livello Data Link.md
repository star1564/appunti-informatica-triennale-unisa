---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
>Il livello **Data Link** è il secondo strato del [[005 Modello ISO-OSI|Modello ISO-OSI]] e si occupa della *trasmissione affidabile dei dati* tra due dispositivi adiacenti e connessi tra loro, rilevando gli errori dei pacchetti dati.

In **trasmissione**, il data link:
1. *riceve i pacchetti dati* dal [[022 Livello di Rete|livello di rete]] e forma i *frame*;
2. aggiunge un *header* e *trailer* per ogni frame, garantendo la consegna affidabile dei dati;
3. imposta i bit per la *[[009 Rilevazione Errori nel Data Link|rilevazione degli errori]]*;
4. assicura il *[[010 Gestione del Flusso nel Data Link|controllo del flusso]]*, limitando la velocità della spedizione dei pacchetti da parte del mittente, evitando di creare carichi troppo grandi da gestire;
5. *passa i frame* al sottostante [[006 Livello Fisico|Livello Fisico]].

In **ricezione**, il data link:
1. *riceve i frame* dal livello fisico;
2. *rimuove* header e trailer da ogni frame;
3. *controlla e gestisce gli errori* di trasmissione;
4. *passa* i dati sotto forma di pacchetti al livello di rete.

![[Pasted image 20230324155332.png]]

Tutte le funzioni di questo livello sono realizzate da un adattatore.

## Framing
Il **Framing** (o incapsulamento) è una tecnica utilizzata nel livello data-link per *suddividere i dati in unità più piccole* chiamate *frame*, e aggiungere un header e un trailer a ciascun frame. Questo processo è importante perché consente di garantire la consegna affidabile dei dati attraverso il canale di comunicazione fisico.

In particolare, il framing *aiuta a distinguere i confini tra i dati trasmessi e i segnali di controllo* che vengono trasmessi sullo stesso canale. Inoltre, suddividere i dati in frame aiuta a gestire la trasmissione di dati di grandi dimensioni, in quanto i frame possono essere trasmessi in sequenza e riuniti alla destinazione finale.

> [!info]+ Header e Trailer
>Gli header e i trailer sono *campi aggiuntivi di informazioni* che vengono aggiunti ai dati durante il processo di framing. 
>
>L'**header** viene aggiunto *all'inizio di ogni frame* e contiene informazioni importanti per la *gestione della trasmissione dei dati*, come ad esempio:
>- l'indirizzo di destinazione e quello di origine; 
>- la lunghezza del frame; 
>- altre informazioni di controllo. 
>
>Queste informazioni sono utilizzate per instradare il frame attraverso la rete e per gestire la trasmissione e la ricezione dei dati.
>
>Il **trailer**, invece, viene aggiunto *alla fine di ogni frame* e contiene informazioni utili al *controllo degli errori sui dati trasmessi*, come ad esempio un [[009 Rilevazione Errori nel Data Link#Checksum|checksum]] o una [[009 Rilevazione Errori nel Data Link#Cyclic Redundancy Code (CRC)|sequenza di controllo dell'errore (CRC)]], che consentono di rilevare eventuali errori di trasmissione del frame. 
>
>Inoltre, il trailer può contenere altre informazioni.

^8afe36

### Tecniche di identificazione frame
La gestione del frame deve prevedere in primo luogo la possibilità del ricevente di identificare il frame, quindi si devono adottare regole per delimitarlo e poterne identificare i limiti in ricezione. Queste tecniche sono:
- *Conteggio dei caratteri*: un campo dell'intestazione indica il numero dei caratteri nel pacchetto, in modo da capire quando finisce e quando inizia il prossimo; ![[Pasted image 20230324163108.png|700]]
- *Indicatori di inizio e fine*: i pacchetti iniziano e terminano con una sequenza speciale di bit, chiamata byte flag, dal valore `01111110`. Per evitare che tale byte sia presente nel contenuto del pacchetto, si inserisce uno `0` dopo ogni gruppo di cinque bit pari ad 1 consecutivi. Tale bit viene eliminato in ricezione; ![[Pasted image 20230324163223.png|750]]
- *Violazione di Codifica*: è possibile segnalare la fine e l'inizio di un pacchetto commettendo una violazione riguardo la [[006 Livello Fisico#^fbfa2b|codifica]] utilizzata. ![[Pasted image 20230324163412.png|700]]


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