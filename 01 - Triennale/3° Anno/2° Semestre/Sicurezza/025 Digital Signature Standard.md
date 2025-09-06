---
aliases:
  - DSS
tags:
  - corsi/informatica/sicurezza
paragrafo: Firme Digitali
cssclasses:
  - 
---
>Il **Digital Signature Standard (DSS)** è una modifica dello schema di [[024 Firme con ElGamal|firme con ElGamal]]. Utilizza delle funzioni [[019 Algoritmi di Hashing Crittografici|hash SHA-1 e SHA-2]] e si basa sempre sull'intrattabilità del problema del [[018 Protocollo Diffie-Hellman#Sicurezza|logaritmo discreto]].

Utilizza due numeri [[042 Numeri Primi|primi]] di lunghezza $L$ ed $N$, dove la lunghezza in bit dello SHA è maggiore o uguale ad $N$, mentre la lunghezza della firma è il doppio di $q$.

> [!question] Sicurezza del DSS
> Si basa sulla difficoltà di calcolare [[018 Protocollo Diffie-Hellman#Sicurezza|logaritmi discreti]].


### Firma con DSE
La chiave *privata* è composta da $$(p,q,\alpha,s)$$
Dove:
- $p$ è un numero primo di $L$ bit
- $q$ è un numero primo di $N$ bit tale che $q|(p-1)$
- $\alpha$ si trova in $Z_n^*$ di ordine $q$, il più piccolo intero positivo $x$ tale che $a^x = 1 \text{ mod } p$
- $s$ è un numero casuale, con $s<q$

La chiave *pubblica* è composta da $$(p,q,\alpha,\beta)$$
Dove:
- $\beta=a^s(\text{mod } p)$ 

Per firmare un messaggio $M$:
- Si sceglie un numero casuale nell'intervallo $[1,q-1]$
- $\gamma=(a^r(\text{mod }  p))\text{ mod }q$
- $\delta=(SHA(M)+s\cdot \gamma)r^{-1}\text{ mod }q)$
- $firma_{(p,q,\alpha,s)}(M,r)=(\gamma,\delta)$

### Verifica con DSE
- $e'=SHA(M)\delta^{-1}\text{ mod }q$
- $e''=\gamma\delta^{-1}\text{ mod }q$

La firma è verificata se $$\gamma=(\alpha^{e'}\beta^{e''} \text{ mod }p)\text{ mod } q$$


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