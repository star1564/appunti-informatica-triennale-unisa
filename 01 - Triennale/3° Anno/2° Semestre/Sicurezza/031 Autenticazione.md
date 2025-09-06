---
aliases:
  - salt
tags:
  - corsi/informatica/sicurezza
paragrafo: Mezzi di Autenticazione
cssclasses:
  - 
---
Per utilizzare un servizio, un utente deve **autenticarsi**. Ci sono diversi modi.


## Password
Il sistema deve memorizzare una *rappresentazione della password* in forma *cifrata*. 

In UNIX, le password si trovano nel file `/etc/passwd`.

![[Pasted image 20240616094653.png|700]]

La funzione di cifratura utilizzata in UNIX è una variante del [[010 Data Encryption Standard|DES]].

![[Pasted image 20240616094822.png|700]]

> [!note]+ Salt
> È un **dato casuale** fornito come input aggiuntivo a una funzione unidirezionale che esegue l'hashing dei dati, una password o una passphrase. 
> 
> L'utilizzo del salt per le password è utile per avere valori cifrati diversi delle password su sistemi diversi (evita collisioni su sistemi diversi).

^cc0c4f


Le password sono vulnerabili a diversi tipi di attacchi:
- intercettazioni
- brute force (il tempo varia in base alla lunghezza della password e dall'intervallo di caratteri usati)
- con dizionario (parole con senso compiuto)

> [!question] Memorizzare le password in forma cifrata con [[013 Advanced Encryption Standard|AES]]
> La password sarà usata come chiave AES e il nome dell'account come testo in chiaro.


## One-time password
È una password che può essere *usata una sola volta*. Può essere implementata attraverso:
- [[019 Algoritmi di Hashing Crittografici|funzioni hash]]
- [[020 Message Authentication Code#HMAC|HMAC]]

## Challenge - Response
L'utente deve rispondere alle *diverse sfide del sistema*. Ad esempio delle domande a cui l'utente ha dato le risposte corrette durante la sua registrazione al sistema.

Ci sono diversi protocolli per questo tipo di autenticazione.

## Two-factor authentication
L'**autenticazione a due fattori** utilizza due (o tre) *fattori* per verificare l'identità dell'utente. 

Richiede *necessariamente* la presenza di autenticazioni *scelte tra fattori diversi*, come:
- email
- password
- codice inviato 
	- sullo smartphone dell'utente
	- su un altro dispositivo fisico

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