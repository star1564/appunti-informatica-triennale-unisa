---
aliases:
  - DES
  - S-box
tags:
  - corsi/informatica/sicurezza
paragrafo: Encryption Standards
cssclasses:
  - 
---
>Il **Data Encryption Standard (DES)** è un algoritmo a [[004 Cifrari Simmetrici|chiave simmetrica]] che segue lo schema del [[009 Cifrari di Feistel|cifrario di Feistel]]. 
>- I dati sono criptati in [[008 Cifrari a Blocchi|blocchi]] da *64 bit* utilizzando una *chiave effettiva da 56 bit*
>- Il suo algoritmo trasforma 64 bit di input in 64 bit di output


![[Pasted image 20240611160827.png|500]]

La *vera chiave è di 64 bit* di cui 8 sono usati per indicare la parità. Infatti, è divisa in 8 byte, dove l'ottavo bit di ciascun byte rappresenta il bit di parità.

![[Pasted image 20240611161122.png|500]]

> [!error] Il DES è stato abbandonato per via della *chiave troppo corta*
> È stato sostituito dall'[[013 Advanced Encryption Standard|AES]].


> [!summary]+ Funzionamento del DES
> Viene passato in input il testo in chiaro da 64 bit e la chiave da 64 bit.
>  
>1. Il testo in chiaro viene modificato in una *[[003 Permutazioni|Permutazione]] Iniziale $(IP)$*
>2. Ci sono 16 round (iterazioni), in cui viene [](003%20Permutazioni.md)che esegue la [[009 Cifrari di Feistel#^d99a1a|sostituzione e permutazione]] attraverso la chiave
>3. Il risultato viene modificato in una *Permutazione Inversa di quella iniziale* $(IP^{-1})$
>
>![[Pasted image 20240611162034.png|500]]

> [!question] Per la decifratura, si utilizza lo stesso algoritmo della cifratura, ma con le sottochiavi in ordine inverso

> [!info]+ Il DES mostra un forte Avalanche Effect
>Un piccolo cambiamento del testo in chiaro oppure della chiave produce un grande cambiamento del testo cifrato.

### Singola Iterazione
Questa è simile a quella nel cifrario di Feistel normale.

$$L_i=R(i-1)\quad\quad R_i=L(i-1) \oplus f(L_i-1, k_i)$$

![[Pasted image 20240611162402.png]]

### La funzione $F$

![[Pasted image 20240611162422.png|500]]

Questa utilizza la espansione $E$ insieme alla permutazione $P$.

#### Espansione $E$
![[Pasted image 20240611162550.png|500]]

#### Permutazione $P$
È una classica permutazione.

![[Pasted image 20240611162625.png|500]]

### Schedulazione delle chiavi
Questa utilizza $PC-1$ e $PC-2$.

![[Pasted image 20240611163431.png|500]]

#### $PC-1$
Viene usata all'inizio della schedulazione sulla chiave originale. 
È una permutazione che rimuove i bit di parità, fornendo in output i 56 bit della chiave.

![[Pasted image 20240611163727.png|500]]

#### $PC-2$
Sta per Compressione e Permutazione. Viene usata prima di fornire una sottochiave. 
Vengono soppressi 8 bit, fornendo in output 48 bit.

![[Pasted image 20240611163848.png|500]]


## S-box
>Una **S-box** (*substitution-box*) è una *tabella non lineare* che esegue la sostituzione. Nei cifrari a blocchi, sono usati per oscurare la relazione tra la chiave ed il testo cifrato, per assicurare la [[009 Cifrari di Feistel#^843541|proprietà di confusione]].

Ogni riga è una permutazione degli interi da 0 a 15.
La S-box ha 6 bit in input, che specificano un elemento della tabella la cui conversione binaria darà i quattro bit in output.

![[Pasted image 20240611163129.png]]

> [!question] Furono progettate per resistere agli attacchi di [[003 Crittografia#^20ecc3|Crittoanalisi]] Differenziale

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