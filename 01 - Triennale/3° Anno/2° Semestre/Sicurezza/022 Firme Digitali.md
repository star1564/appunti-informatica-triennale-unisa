---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Firme Digitali
cssclasses:
  - 
---
>La **Firma Digitale** è un metodo per verificare *[[001 Introduzione alla Sicurezza sui Dati#^61b340|l'autenticità]]* di messaggi o documenti digitali. 
>Una firma digitale valida su un messaggio dà al destinatario la certezza che il messaggio *proviene da un mittente noto* al destinatario.

Questa deve poter essere facilmente prodotta dal legittimo firmatario e *nessun altro utente deve poterla riprodurre*.

![[Pasted image 20240313154115.png|380]]

Utilizza gli [[005 Cifrari Asimmetrici#Cifratura con Chiave Privata|algoritmi di crittografia asimmetrici con chiave privata]] e si divide sempre in *due fasi*:
- La **firma** avviene attraverso la [[005 Cifrari Asimmetrici#^29e17a|Chiave Privata]] dell'utente
- La **verifica** avviene attraverso la [[005 Cifrari Asimmetrici#^29e17a|Chiave Pubblica]] dell'utente che ha firmato il messaggio

> [!info] Firma di messaggi molto lunghi
> Per firmare un messaggio molto lungo, può essere *firmato l'hash del messaggio* ottenuto da una [[019 Algoritmi di Hashing Crittografici|Funzione Hash]].
> Questo consente anche di migliorare l'efficienza e la sicurezza contro alcuni attacchi.

> [!question] La firma digitale di documenti che contengono macro-istruzioni o codice eseguibile non ha valore legale


### Tipi di attacchi
Qui, Alice denota un utente con una firma attaccata da Oscar:
- **Key-only Attack** - Oscar conosce solo la chiave pubblica di Alice
- **Known Message Attack** - Oscar conosce una lista di messaggi e le relative firme di Alice
- **Chosen Message Attack** - Oscar sceglie dei messaggi e chiede ad Alice di firmarli

Una volta che Oscar ha avuto successo nel rompere lo schema della firma, lui può:
- **Total Break** - Determinare la chiave privata di Alice per poter firmare qualsiasi messaggio
- **Selective forgery** - Trovare un algoritmo di firma efficiente che fornisce un modo per costruire firme false su messaggi
- **Existential forgery** - Forgiare una firma per almeno un messaggio.

### Tipi di firme
Si possono realizzare le firme attraverso vari algoritmi:
- [[023 Firme con RSA|Firme con RSA]]
- [[024 Firme con ElGamal|Firme con ElGamal]]
- [[025 Digital Signature Standard|Firme con Digital Signature Standard]]

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
- [Computerphile](https://www.youtube.com/watch?v=s22eJ1eVLTU)