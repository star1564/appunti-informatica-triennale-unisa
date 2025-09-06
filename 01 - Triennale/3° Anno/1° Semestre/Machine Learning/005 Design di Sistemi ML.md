---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Design di Sistemi ML
cssclasses:
  - 
---
>Il primo passo nel **Design di Sistemi ML**, dopo aver considerato *qual è lo scopo del progetto di ML*, è quello di **specificare i requisiti**.

Questi possono variare da sistemi a sistemi, ma dovrebbero essere sempre presenti le seguenti caratteristiche:
- **Affidabilità**: permette al *sistema di continuare a funzionare* al livello desiderato di performance anche *durante dei problemi* (come problemi hardware o software, o errore umano).
	- Il problema principale per questa caratteristica è che i sistemi ML possono *fallire silenziosamente* nelle predizioni. Dando, appunto, delle predizioni errate e a volte difficili da individuare.
- **Scalabilità**: un sistema di ML può *crescere* in *complessità, in traffico di utenti e in numero di modelli*. Quindi bisogna pensare a come gestire questa crescita, sia dell'*utilizzo delle risorse fisiche* che del *modello stesso*. 
	- Una soluzione sarebbe quella dell'*up-scaling* (espansione delle risorse per gestire la crescita) e *down-scaling* (riduzione delle risorse quando non sono richieste).
- **Mantenibilità**: facile manutenzione del modello;
- **Adattabilità**: il modello (e quindi anche il codice) si deve adattare al cambiamento dei dati.

## Processo di design iterativo
Sviluppare un sistema ML può essere un processo *iterativo*, e nella maggior parte dei casi, *senza fine*. Una volta messo in produzione, il modello deve essere costantemente monitorato e aggiornato.

![[Pasted image 20230930121031.png|500]]

1. *Project Scoping*: Un progetto inizia con la definizione degli obiettivi e vincoli. Le parti interessate devono essere identificate e coinvolte. Le risorse devono essere stimate e assegnate.
2. *Data Engineering*: La maggior parte dei modelli di ML oggi apprende dai dati, quindi lo sviluppo di modelli di ML inizia con la gestione di dati provenienti da fonti e formati diversi. Una volta ottenuto l'accesso ai dati grezzi, si dovranno raccogliere i dati per l'addestramento, campionandoli e generando etichette.
3. *ML model development*: Con l'insieme iniziale di dati di addestramento, dovremo estrarre le caratteristiche e sviluppare i modelli iniziali. Lo si fa mediante la selezione, l'addestramento e la valutazione del modello.
4. *Deployment*: Una volta sviluppato, il modello deve essere reso accessibile agli utenti.
5. *Monitoring and continual learning*: Una volta in produzione, i modelli devono essere monitorati per verificare il decadimento delle prestazioni e mantenuti per adattarsi ad ambienti e requisiti in continua evoluzione.
6. *Business analysis*: La performance del modello deve essere valutata in base agli obiettivi aziendali e analizzata per generare "intuizioni" aziendali. Queste intuizioni possono poi essere utilizzate per eliminare progetti non produttivi o per definire i contorni di nuovi progetti. Questo passo è strettamente correlato al primo passo.




___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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