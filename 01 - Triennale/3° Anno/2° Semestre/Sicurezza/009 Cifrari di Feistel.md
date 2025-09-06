---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Cifrari a Blocchi
cssclasses:
  - 
---
>Un **Cifrario di Feistel** consiste nel combinare un certo numero di *cifrari semplici in una struttura iterativa*, sfruttando il concetto di **cifrario prodotto**, con l'obiettivo di offrire una sicurezza maggiore rispetto a ciascun cifrario individuale.
>
>Questi tipi di cifrari utilizzano *lo stesso algoritmo per la cifratura e la decifratura*.

L'algoritmo si basa su due principi chiave:
- **Diffusione** - Il cifrario mira a dissipare le informazioni statistiche del testo in chiaro all'interno del testo cifrato, rendendo difficile la [[003 Crittografia#^20ecc3|crittoanalisi]]. La distribuzione delle frequenze dei bit nel testo cifrato dovrebbe essere il più casuale possibile, indipendentemente dal testo in chiaro.
- **Confusione** - La relazione tra il testo in chiaro e il testo cifrato dovrebbe essere la più complessa possibile, data la chiave segreta. ^843541

L'algoritmo di cifratura ha una *struttura iterativa*, composta da $n$ **round** (o iterazioni). 
Ogni round applica la stessa funzione $F$ (chiamata *round function*) a una porzione del blocco di testo in chiaro, utilizzando una *sottochiave* derivata dalla chiave segreta principale (ottenuta dall'algoritmo di schedulazione della chiave). 

> [!summary]+ Svolgimento del ciframento
>1. Il blocco di testo in chiaro viene diviso in due metà $L_0$ e $R_0$.
>2. 🔁Per ogni round $i$ (da $1$ a $n$):
>	- La metà *destra* del blocco dati $R_{i-1}$ e la *sottochiave* $K_i$ vengono passate alla funzione $\color{#61AFEF}F$, producendo un risultato $F(R_{i-1}, K_i)$
>	- Si esegue l'operazione [[029 XOR (AdE)|XOR]] tra $F(R_{i-1}, K_i)$ e la metà sinistra del blocco dati $L_{i-1}$, ottenendo una *nuova metà destra* $R_i$
>	- La *nuova metà sinistra* $L_i$ diventa $R_{i-1}$
>3. Le due *metà finali* del blocco dati $L_n$ e $R_n$ vengono *scambiate e combinate* per ottenere il blocco di testo cifrato.
>
>![[Pasted image 20240611152121.png|500]]

^d99a1a

Per la decifratura, si utilizza lo stesso algoritmo della cifratura, ma con le sottochiavi in ordine inverso.

> [!example]- <font color="orange">Esempio</font>
>Cifratura:
>![[Immagine 2024-06-11 151808.jpg]]
>
>Decifratura:
>![[Pasted image 20240611152239.png]]

Uno degli esempi di cifrari di Feistel è il [[010 Data Encryption Standard|Data Encryption Standard (DES)]]. 

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