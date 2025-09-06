---
aliases:
  - Steganografia
  - Crittoanalisi
  - Brute Force
tags:
  - corsi/informatica/sicurezza
paragrafo: Crittografia
cssclasses:
  - 
---
>La **Crittografia** (*Cryptography*) è una branca della matematica che si occupa della *trasformazione dei dati*. Gli **algoritmi crittografici** sono utilizzati in molti modi per la sicurezza delle informazioni e delle reti. La crittografia è una componente essenziale per l'archiviazione e la *trasmissione sicura dei dati* e per l'interazione sicura tra le parti.

![[Pasted image 20240313140856.png]]

Gli algoritmi crittografici possono essere suddivisi in tre categorie, in base al numero di **chiavi** utilizzate:
- [[004 Cifrari Simmetrici|Algoritmi Single-Key (Cifrari Simmetrici)]]
- [[005 Cifrari Asimmetrici|Algoritmi Two-Key (Cifrari Asimmetrici)]]
- [[006 Algoritmi senza Chiave|Algoritmi Keyless]]

> [!info] Steganografia
> La crittografia si differenzia dalla steganografia, perché quest'ultima si occupa di *occultare i dati sensibili*. Quindi la segretezza è perduta al momento dell'intercettazione.



## Attaccare un sistema di crittografia
In genere, l'obiettivo di un attacco a un sistema di crittografia è quello di *recuperare la chiave in uso*, piuttosto che recuperare semplicemente il testo in chiaro di un singolo testo cifrato. 

Esistono due approcci generali per attaccare uno schema di crittografia convenzionale:
- **Crittoanalisi** - Gli attacchi crittoanalitici si basano *sulla natura dell'algoritmo* e forse *sulla conoscenza delle caratteristiche generali del testo in chiaro* o anche su alcune coppie campione di testo in chiaro e testo cifrato. Questo tipo di attacco sfrutta le caratteristiche dell'algoritmo per *tentare di dedurre* un testo in chiaro specifico o per dedurre la chiave utilizzata. ^20ecc3
	- *Known Ciphertext Attack*: L'avversario conosce solo il testo cifrato
	- *Known Plaintext Attack*: L'avversario conosce anche il testo in chiaro
	- *Chosen Plaintext Attack*: L'avversario può ottenere la cifratura di testi in chiaro di sua scelta
	- *Chosen Ciphertext Attack*: L'avversario può ottenere la decifratura di testi cifrati di sua scelta
- **Brute Force** - L'attaccante tenta di *utilizzare tutte le chiavi possibili* su un testo cifrato *fino a ottenere una traduzione leggibile* in testo in chiaro. In media, è necessario provare la metà di tutte le chiavi possibili per ottenere un successo. ^9a60da
	- Il tempo necessario per eseguire questo tipo di attacco *varia in base al tipo di algoritmo di cifratura utilizzato*: più è complicato, più è improponibile il tempo di risoluzione. Molto spesso gli algoritmi hanno [[054 Tempo Polinomiale ed Esponenziale|Tempo Esponenziale]].

Se uno dei due tipi di attacco riesce a dedurre la chiave *tutti i messaggi futuri e passati* criptati con quella chiave sono compromessi.

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