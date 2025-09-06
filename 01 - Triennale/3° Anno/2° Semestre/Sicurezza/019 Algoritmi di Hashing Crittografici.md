---
aliases:
  - Funzione Hash Crittografica
  - SHA
  - MD5
tags:
  - corsi/informatica/sicurezza
paragrafo: Algoritmi di Crittografia
cssclasses:
  - 
---
>Un tipo importante di [[006 Algoritmi senza Chiave|algoritmo senza chiave]] è la **[[Funzione Hash]] Crittografica**. Viene usata come codice di autenticazione per: 
>- Messaggi, documenti e file
>- [[022 Firme Digitali|Firme digitali]]
>- [[026 Certificati|Certificati]]

Quindi il valore hash della funzione $hash(M)$, è una rappresentazione *non ambigua e non falsificabile* del messaggio $M$. Infatti, se il messaggio $M$ cambia, cambierà anche l'hash di $M$.

Le funzioni più comuni sono:
- **MD5** (*Message Digest Algorithm*), con valore di 128 bit. Questo algoritmo è *outdated* in quanto contiene collisioni.
- **SHA-0 e SHA-1** (*Secure Hash Algorithm*), con valore di 160 bit. Questi algoritmi hanno problemi di sicurezza.
- **SHA-2**, con valore di 224-256-384-512 bit

> [!note]+ Sicurezza delle funzioni hash
>- **Sicurezza debole**: dato un messaggio $M$, è computazionalmente difficile trovare un altro messaggio $M'$ tale che $hash(M) = hash(M')$
>	- *Problema*: Dato $M$, trovare $M'$. 
>	- *Conosciuto*: Un messaggio specifico $M$.
>	- Si assume quindi che si parta da un messaggio esistente.
>- **Sicurezza forte**: è computazionalmente difficile trovare due diversi messaggi con lo stesso valore hash
>	- *Problema*: Trovare due messaggi $M$ ed $M'$. 
>	- *Conosciuto*: Nessun messaggio specifico a priori.
>	- Si cerca quindi qualsiasi coppia di messaggi con lo stesso hash.
>- **One Way**: dato un valore hash $y$ è computazionalmente difficile trovare il messaggio originale $M$ tale che $y=hash(M)$

> [!question] Paradosso del compleanno
> Il paradosso del compleanno è utile per analizzare la probabilità di successo di trovare collisioni nelle funzioni hash.


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
- [Computerphile](https://www.youtube.com/watch?v=b4b8ktEV4Bg)