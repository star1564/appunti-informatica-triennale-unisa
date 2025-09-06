---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Firme Digitali
cssclasses:
  - 
---
Il sistema di crittografia a chiave pubblica **[[017 RSA|RSA]]** fornisce uno schema di **[[022 Firme Digitali|firma digitale]]**, è basato sulla matematica delle [[018 Protocollo Diffie-Hellman#^82060c|esponenziazioni modulari]] e dei [[018 Protocollo Diffie-Hellman#Sicurezza|logaritmi discreti]] e sulla difficoltà computazionale del problema RSA (e del relativo problema di [[017 RSA#Problema della Fattorizzazione|fattorizzazione]] degli interi).

### Firma con RSA
Dopo aver aver generato la chiave pubblica e privata con RSA, si vuole **firmare** un messaggio $M$ *attraverso la chiave privata* $d$:
1. Si calcola *l'[[019 Algoritmi di Hashing Crittografici|hash]] del messaggio*: $h=hash(M)$
2. *Criptare l'hash* del messaggio attraverso la *chiave privata* per calcolare la firma: $F=M^d(\text{mod } n)$

![[Pasted image 20240614125425.png]]

Dove: 
- $0\leq h \le n$
- $0\le F < n$

> [!question]+ Corretta generazione dei parametri per la firma digitale RSA
> Questa procedura è uguale a quella nell'RSA. Dato un input $L$:
> 1. Generare due numeri primi $p,q$ di lunghezza $L/2$
> 2. Calcolare $n=pq$
> 3. Scegliere un $e$ tale che $gcd(e,\phi(n))=1$, ovvero il Greatest Common Divisor ([[045 Massimo Comun Divisore|MCD]])
> 4. Scegliere come inverso moltiplicativo di $e\mod (p-1)(q-1)$
> 5. La chiave pubblica è $(n,e)$ e la chiave privata è $(n,d)$


> [!note]+ Public-Key Cryptography Standards per la **FIRMA** (PKCS #1)
> > [!warning] Da non confondere con quello [[017 RSA#^f71696|per la cifratura]]
> > Quello per la firma utilizza il nome RSA_SSA (Signature Scheme with Appendix), si calcola subito un hash del messaggio e poi lo si firma.
> 
> In esso sono descritti due schemi per la *firma*:
>- **RSA_SSA-PKCS1-v1_5**: Non ci sono prove di sicurezza per questo schema, ma *neanche si conoscono attacchi* efficienti.
>- **RSA_SSA-PSS**: È basato sul *Probabilistic Signature Scheme* e consiste nell'Hash del messaggio con un [[031 Autenticazione#^cc0c4f|salt]] casuale. Per via del salt, un fissato messaggio ed una chiave pubblica, la firma *non è sempre la stessa*.

^3198fd


### Verifica della firma con RSA
Si vuole **verificare** la firma $F$ per il messaggio $M$ *attraverso la chiave pubblica* $e$:
1. Si calcola *l'[[019 Algoritmi di Hashing Crittografici|hash]] del messaggio originale*: $h=hash(M)$
2. Si *decifra la firma*: $h'=F^e(\text{mod } n)$
3. Se $h=h'$, allora la firma è valida. Non lo è altrimenti.

![[Pasted image 20240614125923.png]]

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
- https://cryptobook.nakov.com/digital-signatures/rsa-signatures