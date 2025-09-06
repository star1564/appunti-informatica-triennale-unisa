---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Crittografia
cssclasses:
  - 
---
>Gli **Algoritmi di Crittografia Simmetrica** (single-key) dipendono dall'uso di una *singola chiave segreta*. Prende in *input* alcuni dati da proteggere e una chiave segreta e produce una *trasformazione incomprensibile* su tali dati.
>
>Un *algoritmo di decifrazione corrispondente* prende i dati trasformati e la stessa chiave segreta e recupera i dati originali. 

![[Pasted image 20240313143013.png|800]]

Un cifrario simmetrico è composto da:
- **Dati in chiaro** (*plaintext*) - Il messaggio originale e *leggibile*.
- **Dati cifrati** (*ciphertext*) - Il messaggio *non leggibile*.
- **Algoritmo di Crittografia** (*encryption algorithm*) - È l'algoritmo che *trasforma* i dati in chiaro in dati cifrati.
- **Chiave Segreta** - È un valore *indipendente dal testo in chiaro e dall'algoritmo*. L'algoritmo produrrà un *risultato diverso a seconda della chiave* specifica utilizzata in quel momento. Le sostituzioni e le trasformazioni esatte eseguite dall'algoritmo dipendono dalla chiave.
- **Algoritmo di Decifrazione** (*decryption algorithm*) - Si tratta dell'algoritmo di crittografia eseguito al *contrario*. Prende il testo cifrato e la chiave segreta e produce il testo in chiaro originale.

La chiave può essere nota a:
- *Un singolo utente* - ad esempio per proteggere i dati memorizzati che saranno accessibili solo al creatore dei dati.
- *Due parti* - in modo da proteggere la comunicazione tra di loro.
- *Più di due utenti* - in questo caso, l'algoritmo protegge i dati da coloro che condividono la chiave al di fuori del gruppo.

La crittografia simmetrica assume le seguenti forme: 
- **[[008 Cifrari a Blocchi|Cifrario a blocchi]]**
- **[[014 Cifrari a Stream|Cifrario a stream]]**

___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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