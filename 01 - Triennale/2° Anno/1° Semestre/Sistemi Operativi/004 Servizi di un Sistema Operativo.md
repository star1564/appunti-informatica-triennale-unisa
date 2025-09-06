---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Basi di un Sistema Operativo
cssclasses:
  - 
---
Un sistema operativo offre un ambiente in cui eseguire i programmi e fornire **servizi**. Ovviamente, i servizi specifici variano secondo il sistema operativo, ma si possono identificare alcune classi di servizi comuni.

![[Pasted image 20220929093122.png|800]]
<center>Panoramica dei servizi del sistema operativo</center>

#### SERVIZI PER L'UTENTE GENERICO
Un primo insieme di servizi offre funzionalità utili all'utente.

- **Interfaccia con l'utente**: essa può assumere diverse forme, ma quella più comune è l'[[Interfaccia GUI|interfaccia utente grafica (GUI)]]. Un'altra possibilità è l'[[Interfaccia CLI|interfaccia a riga di comando (CLI)]]. Certi sistemi offrono entrambe le soluzioni;

- **Esecuzione di un programma**: il sistema deve poter caricare un programma in memoria ed *eseguirlo*. Questo può terminare la sua esecuzione in modo normale o anomalo;
- **Operazioni di I/O**: un programma in esecuzione può richiedere un'operazione di *I/O* che implica l'uso di un file o di un dispositivo di I/O;
- **Gestione del file system**: I programmi richiedono l'esecuzione di *operazioni* di lettura e scrittura su file, creazione e cancellazione di directory, ricerca dei file. Il sistema operativo gestisce questo e anche i permessi di accesso ai file;
- **Comunicazioni**: I processi possono *scambiarsi informazioni*, sullo stesso computer o tra computer su una rete;
- **Rilevamento di errori**: Il sistema operativo deve essere *costantemente informato sugli errori* che possono derivare da Hardware e Programmi, prendendo azioni adeguate per assicurare computazioni corrette e consistenti.

#### SERVIZI IMPLICITI
Un secondo gruppo di funzioni del sistema operativo non riguarda direttamente gli utenti, ma assicura il funzionamento efficiente del sistema stesso.

- **Allocazione delle risorse**: se sono attivi più utenti o sono contemporaneamente in esecuzione più processi, il sistema operativo  provvede all'*allocazione delle risorse necessarie* (cicli CPU, memoria, file, dispositivi I/O) a ciascuno di essi;

- **Logging** (oppure accounting, contabilizzazione): Vogliamo tenere traccia di quali programmi ed utenti usano il calcolatore, segnalando *quali e quante risorse impiegano*;
- **Protezione e Sicurezza**: I proprietari di dati memorizzati in un sistema multiutente o connesso alla rete desiderano controllare l'uso dei propri dati ed essere sicuri che i processi non interferiscano tra di loro. La *protezione* assicura che l'accesso alle risorse del sistema sia controllato, mentre la *sicurezza* autentifica gli utenti e difende la macchina da attacchi esterni.

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

Altri collegamenti: 
