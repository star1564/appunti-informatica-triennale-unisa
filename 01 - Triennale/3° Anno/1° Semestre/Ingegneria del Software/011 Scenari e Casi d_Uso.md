---
aliases:
  - Scenario
  - Caso d'Uso
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Estrazione
cssclasses:
  - 
---
>**Scenari** e **Casi d'Uso** sono entrambi strumenti utilizzati per *descrivere come degli [[005 UML#Attori|Attori]] interagiranno con un sistema*. Sono utili per unire le idee dei clienti con quelle degli sviluppatori. Entrambi utilizzano un *linguaggio naturale*.

Tuttavia, ci sono alcune differenze chiave tra i due.

## Scenario
> Uno **Scenario** è un *esempio di utilizzo di un certo numero di funzionalità* del sistema dal punto di vista di uno o più attori. 
> È una *descrizione concreta, informale e narrativa* di un evento e mostra come gli attori provano ad usare il sistema.

> [!tip] Non sono necessari molti scenari
> Gli scenari *non sono una rappresentazione completa* di un sistema software e non possono catturare completamente tutte le sue funzionalità. Infatti, questi si concentrano solamente su dei particolari eventi.

Un singolo scenario *può contenere una sola decisione* (outcome). Se un evento ha molteplici outcome, è necessario creare uno scenario per ogni outcome.

Ogni scenario deve essere caratterizzato da:
- un **nome**;
- una **lista di partecipanti**;
- un **flusso di eventi**.

Ci possono essere diversi tipi di scenari:
- *As-is Scenario*, descrivono la situazione *corrente*. Durante il [[009 Estrazione dei Requisiti#^d7a017|re-engineering]], il sistema attuale è definito dall'osservazione degli utenti e dalla descrizione delle loro azioni all'interno dello scenario;
- *Visionary Scenario*, descrivono un *sistema da costruire*. Usati durante il [[009 Estrazione dei Requisiti#^17dc3c|greenfield engineering]].
- *Evaluation Scenario*, descrivono le attività degli utenti e le mettono a confronto con il sistema da *valutare*.
- *Training Scenario*, sono dei tutorial per l'introduzione di nuovi utenti al sistema.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20231007101943.png]]
>Da notare che:
>- L'emergenza in questo scenario è un incendio;
>	- In un altro scenario, l'emergenza sarebbe potuta essere qualcos'altro (come una rapina).
>- Alice richiede dei paramedici e dei pompieri allo stesso tempo.
>	- In un altro scenario (come una rapina) avrebbe potuto richiedere la polizia e dei paramedici.

## Caso d'Uso
>Un **Caso d'Uso** è una astrazione che *descrive una completa serie di eventi* che avvengono dopo un'inizializzazione da parte di un attore. Specifica tutti i possibili scenari per una determinata funzionalità. In pratica è una *generalizzazione* di uno scenario.

Un caso d'uso può avere diversi outcome, dove ogni outcome può essere uno specifico scenario. 
Si può dire, quindi, che uno scenario sia una *istanza* di un caso d'uso.

Dopo aver trovato uno scenario, si deve trovare il suo caso d'uso inerente.
Il *Modello dei Casi d'Uso* è l'insieme di tutti i casi d'uso che specificano le complete funzionalità del sistema.

Ogni caso d'uso *eredita le proprietà degli scenari*, aggiungendo:
- una **Entry Condition**, le condizioni in cui si entra nel caso d'uso;
- una **Exit Condition**, le condizioni in cui si esce dal caso d'uso;
- dei **Requisiti Non Funzionali** e **Vincoli**;
- le **Eccezioni** che possono avvenire.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20231007103114.png]]
>Nel caso d'uso, il problema del tipo dell'emergenza e dei rinforzi richiesti è generalizzato ad un semplice form che viene compilato dal Field Officer.
>
>![[Pasted image 20240117154200.png|1000]]

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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