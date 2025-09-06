---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Basi di un Sistema Operativo
cssclasses:
  - 
---
>Un **Sistema Operativo** (*SO, Operative System, OS*) è un software di base che gestisce le risorse hardware e software della macchina. ^3c6e13

Opera da *intermediario* tra l'utente e l'hardware del computer, eseguendo  programmi e facilitando l'utilizzo della macchina offrendo un ambiente d'uso conveniente.

#### MODERNO SISTEMA DI CALCOLO

![[Pasted image 20220921103447.png|600]]

Un sistema di calcolo è formato dai seguenti componenti:
- *Hardware*: sono i dispositivi fisici all'interno del computer che forniscono risorse computazionali di base (CPU, memoria, I/O device);
- *Sistema Operativo*: controlla e coordina l'uso dell'hardware tra applicazioni e utenti (Windows, Linux, Mac, ...); ^6173a3
- *Programmi*: definiscono i modi attraverso i quali le risorse del sistema vengono utilizzate per risolvere problemi computazionali degli utenti (editor di testo, compilatore, browser web, ...);
- *Utenti*: Possono essere persone, dispositivi oppure altri computer.

![[Pasted image 20220921105738.png|700]]

La CPU e controller di dispositivi sono connessi ad un [[Bus#Bus|bus]] comune che fornisce accesso alla memoria condivisa.

Ogni controller che gestisce un particolare tipo di dispositivo possiede un [[Buffer|Buffer locale]]. La CPU trasferisce dati dalla/alla memoria in/da buffer locali. I controller dei dispositivi informano la CPU che hanno finito il proprio lavoro generando un [[Interrupt e Trap|interrupt]].

---
#### IL RUOLO DEL SISTEMA OPERATIVO

Il ruolo del Sistema Operativo cambia in base alla macchina su cui è installato (PC: per un singolo utente; Mainframe: più utenti condividono le stesse risorse; ...).

- *Alloca risorse* decidendo come assegnare equamente ed efficientemente le risorse ai programmi;
- *Controlla* che i programmi funzionino correttamente e senza usi impropri del computer (come quando non si ha il permesso di modificare file);
- *Esegue programmi* caricandoli in memoria e terminandoli in mono normale o anomalo.

[[004 Servizi di un Sistema Operativo|Altro sui servizi di un sistema operativo]].

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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

<center><font color="rainbow"><i>"Caution: Handle With Care!<br>Removal of the OS chip will result in death."</i></font></center>


Altri collegamenti: 




