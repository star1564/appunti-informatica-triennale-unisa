---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Basi della Programmazione ad Oggetti
cssclasses:
  - 
---
La **Programmazione Orientata ad Oggetti** (POO / OOP) è un paradigma di programmazione che permette di definire [[Oggetti]] che possono contenere dei dati, sotto forma di campi definiti da una [[Classi|Classe]], e che possono essere modificati attraverso [[Metodi]].

![[Pasted image 20220921142237.png|650]]
<center>Esempio di uno Schema di una Classe : Libro</center>

---
### PRINCIPI DI PROGRAMMAZIONE
I principi ispiratori di un buon progetto software sono:
- **Astrazione**, procedimento mentale che consente di:  ^1dfc00
	- *Evidenziare le caratteristiche* pregnanti di un problema;
	- Offuscare o ignorare gli aspetti che si ritengono secondari rispetto ad un determinato obbiettivo.
- **Information Hiding**, *nascondere il funzionamento interno* (deciso in fase di progetto) di una parte di un programma;
	- Usato spesso per rendere inaccessibili parti del programma importanti;
	- Ha anche lo scopo di far lavorare determinate funzioni solo su specifici dati, per fare ciò nel C si usano gli [[016 Introduzione agli ADT|ADT]];.
- **Riuso del Codice**, pratica di *richiamare* o invocare parti di codice precedentemente già scritte ogni qualvolta risulta necessario, senza doverle scrivere daccapo (Funzioni e Librerie di Funzioni);
- **[[004 Programmazione Modulare|Modularità]]**, tecnica di suddividere un progetto software per gestirne meglio la complessità.

Per tenere in mente tutto quello che si deve fare durante l'analisi di un problema e la costruzione di un algoritmo si usa un [[003 Fasi dello Sviluppo del Programma#DIZIONARIO DEI DATI|Dizionario dei Dati]].

---
### MODELLI DI LINGUAGGI
#### AD INTERPRETAZIONE
1. Si considera una istruzione per volta nel codice sorgente espresso in un linguaggio di programmazione ad alto livello;
2. Si traduce l'istruzione considerata in una o più istruzioni in linguaggio macchina, che vengono immediatamente eseguite;
3. Si passa a considerare l'istruzione successiva.

Questo metodo è leggermente più lento, ma se si verifica un errore si sa esattamente qual è il problema.

#### A COMPILAZIONE
La compilazione consiste nel tradurre l'intero programma in linguaggio di alto livello prima di poterlo eseguire.

___
[[000 Indice POO|↖ Ritorna all'indice ↖]]

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
#### COLLEGAMENTI UTILI DA PSD:
- [[004 Programmazione Modulare|Programmazione Modulare]];
- [[003 Fasi dello Sviluppo del Programma|Fasi dello Sviluppo di un Programma (Sintattica, Semantica, Dizionario dei Dati)]];
- [[016 Introduzione agli ADT|Introduzione agli ADT (Specifica ADT)]];