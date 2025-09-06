---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Interi
cssclasses:
  - 
---
## Rappresentazione
### Numero di Bit in una Sequenza a Binario Puro
In **Binario Puro**, un intero può essere rappresentato da *n* bit, più bit ci sono, più grande è il numero che possiamo rappresentare.

Con n bit possiamo rappresentare tutti i *$2^n$* interi compresi nell'**intervallo di rappresentazione**:
**$$[0, +2^{n}-1]$$**

Se con k bit posso rappresentare t sequenze distinte, con *k+1* bit posso rappresentare il doppio di sequenze distinte (*2t*).

Il minimo numero n di bit necessari a rappresentare N in binario è tale che:
**$$N<2^n$$**

### Scrivere in ordine una sequenza di bit
Per scrivere in ordine una sequenza di bit si considera il loro [[Bit Più Significativo]].
- Si prende il numero di bit *n* e si calcola il numero di sequenze (*$2^{n}$*);
- Questo numero lo si divide per 2;
- La prima parte sarà dedicata a tutte le combinazioni che cominciano con 0, la seconda con 1.
- Ripeti con il prossimo bit.

> [!example]- <font color="orange">Esempio</font>
>3 bits, $2^3=8$ combinazioni possibili:
>
>*0*00
>*0*01
>*0*10
>*0*11
>
>**1**00
>**1**01
>**1**10
>**1**11

### Numeri Pari e Dispari in Binario
I numeri *pari* si riconoscono perché hanno sempre **0** al [[Bit Meno Significativo]].
Per esempio: (1101**0**)$_2$ è pari.

I numeri *dispari* si riconoscono perché hanno sempre **1** al [[Bit Meno Significativo]].
Per esempio: (1101**1**)$_2$ è dispari.

## Conversione

I numeri interi possono essere convertiti tra tutte le basi che vedremo:
- [[003 Base Qualunque e Decimale#BASE QUALUNQUE → DECIMALE|Base Qualunque → Decimale (Q → 10)]];
	- [[003 Base Qualunque e Decimale#DECIMALE → BASE QUALUNQUE|Decimale → Base Qualunque (10 → Q)]].
 $\quad$
- [[004 Ottale e Binario#OTTALE → BINARIO|Ottale → Binario (8 → 2)]];
	- [[004 Ottale e Binario#BINARIO → OTTALE|Binario → Ottale (2 → 8)]];
 $\quad$
- [[005 Esadecimale e Binario#ESADECIMALE → BINARIO|Esadecimale → Binario (16 → 2)]];
	- [[005 Esadecimale e Binario#BINARIO → ESADECIMALE|Binario → Esadecimale (2 → 16)]];
 $\quad$
- [[006 Ottale ed Esadecimale#OTTALE → ESADECIMALE|Ottale → Esadecimale (8 → 16)]]
	- [[006 Ottale ed Esadecimale#ESADECIMALE → OTTALE|Esadecimale → Ottale (16 → 8)]]

___

[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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