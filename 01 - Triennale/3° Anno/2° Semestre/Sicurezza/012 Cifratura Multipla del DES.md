---
aliases:
  - DES Doppio
  - 2DES
  - DES Triplo
  - 3DES
tags:
  - corsi/informatica/sicurezza
paragrafo: Encryption Standards
cssclasses:
  - 
---
Nel [[010 Data Encryption Standard|DES]] la chiave è piccola (56 bit). 

>Attraverso la **Cifratura Multipla** è possibile costruire un cifrario *più sicuro* a partire dal DES, senza modificarne la struttura. Consiste nel cifrare il messaggio varie volte *con chiavi differenti*.

## Doppio DES

>Il **Doppio DES** utilizza *due istanze di DES* sullo stesso testo in chiaro. In entrambe le istanze utilizza *chiavi diverse*, che vengono usate anche per la decifrazione.

![[Pasted image 20240611174428.png]]

Il testo normale a 64 bit entra nella prima istanza DES che viene poi convertita in un testo intermedio a 64 bit utilizzando la prima chiave e quindi passa alla seconda istanza DES che fornisce testo cifrato a 64 bit utilizzando la seconda chiave.

Il doppio DES utilizza una chiave a 112 bit ma fornisce un *livello di sicurezza (di tempo) di $2^{56}$* e non $2^{112}$. Questo è dovuto all'attacco meet-in-the-middle che può essere utilizzato per rompere il doppio DES.

> [!note]+ Meet-in-the-middle attack
> Si conosce solamente $(P,K)$.
>- L'attaccante calcola in anticipo tutte le possibili combinazioni di $K_1$ per ogni blocco di testo in chiaro. Questo produce un enorme "dizionario" di valori intermedi.
>- L'attaccante intercetta un messaggio cifrato con Doppio DES. Prova a decifrarlo solo con la seconda chiave $K_2$ per tutti i possibili valori.
>- Confronta i risultati parziali (decifrati con $K_2$) con il "dizionario" calcolato in precedenza (cifrati con $K_1$). Se trova una corrispondenza, ha scoperto la chiave $(K_1,K_2)$.


## Triplo DES
>Il **Triplo DES** utilizza *tre istanze di DES* sullo stesso testo in chiaro. Utilizza tre chiavi diverse.

![[Pasted image 20240611175906.png]]

Ha un livello di sicurezza di $2^{112}$, in quanto l'attacco con il meet-in-the-middle può essere eseguito in due punti "centrali".

![[Pasted image 20240611175919.png]]


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
- https://www.geeksforgeeks.org/double-des-and-triple-des/