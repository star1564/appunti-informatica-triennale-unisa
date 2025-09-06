---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Rete
cssclasses:
  - 
---
## Router
>Un **Router** può *interconnettere reti* che usano diverse tecnologie, inclusi mezzi fisici diversi, tecniche di accesso, schemi di indirizzamento fisico e formato dei frames.

Un router monta un [[001 Introduzione a SO#^3c6e13|sistema operativo]] nella sua memoria chiamato **Cisco IOS** ed è caratterizzato da: 
- *porte di ingresso ed uscita*; 
- un blocco di commutazione che collega le porte di ingresso con quelle di uscita; 
- un processore di instradamento che esegue protocolli di [[026 Routing e Forwarding|routing]] e aggiorna le tabelle di routing; 
- varie [[070 Gerarchia di Memoria#PARAMETRI E GERARCHIA|memorie]].

## Autonomous System
>Il collegamento di più reti *sotto un unico dominio* prende il nome di **Autonomous System** (*AS*). 

### Tipologie di AS
I router che instradano messaggi nello *stesso AS* e che non hanno diretta connessione con i router di altre reti sono detti **Interior Router**. Scambiano informazioni di instradamento tramite un Interior Gateway Protocol (IGP).

![[Pasted image 20230529104044.png|550]]

I router che instradano messaggi tra *AS diversi* sono detti **Exterior Router**. Scambiano informazioni utilizzando un Exterior Gateway Protocol (EGP).

![[Pasted image 20230529104212.png|550]]

I router che hanno la funzione di fare da *ponte di collegamento tra AS diversi* vengono detti **Border Router** (anche router di frontiera).

![[Pasted image 20230529104246.png|550]]

### Peering
Il **Peering** è la *connessione tra due AS appartenenti a provider distinti*. L'instradamento tra due AS avviene sempre nello stesso modo, con la differenza che esiste un unico algoritmo di instradamento tra organizzazioni adiacenti ed è necessario aggiornare le tabelle di routing manualmente aggiungendo percorsi statici. 

![[Pasted image 20230529112322.png]]

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
