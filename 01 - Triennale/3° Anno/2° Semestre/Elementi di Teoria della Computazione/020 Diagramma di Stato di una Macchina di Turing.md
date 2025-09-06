---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
  - 
---
Come nel caso degli automi, spesso preferiamo utilizzare il diagramma di stato di una specifica Macchina di Turing piuttosto che la [[018 Macchine di Turing#Definizione Formale|descrizione formale della settupla]]. 

>Il **diagramma di stato di una Macchina di Turing** è un [[012 Grafi|Grafo]] i cui nodi hanno come nomi gli stati della macchina. Le etichette degli archi hanno la forma 
>$$\textcolor{#00C575}{simbolo} \to \textcolor{#FF6611}{simbolo}, \textcolor{#e8e42c}{dir}$$
>dove: 
>- $simbolo\in\Gamma$
>- $dir \in \{L, R\}$

- Il <font color="#00C575">simbolo</font> a sinistra della freccia rappresenta il *simbolo letto*
- Il <font color="#FF6611">simbolo</font> a destra della freccia rappresenta il *simbolo che sostituisce il simbolo letto* (il simbolo può essere eventualmente lo stesso) per effetto della [[019 Funzione di Transizione delle Macchine di Turing|transizione]]
- Il simbolo $\color{#e8e42c}dir$ rappresenta *la direzione dello spostamento* per effetto della transizione

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240422123558.png]]
>
>Vede se una stringa è $a^*$.
>
>- $a\to a, R$ sullo stato $q_0$ equivale a $\delta(q_0,a)=(q_0,a,R)$
>- $b\to b, R$ sullo stato $q_0$ equivale a $\delta(q_0,b)=(q_{reject},b,R)$
>- $\sqcup\to \sqcup, L$ sullo stato $q_0$ equivale a $\delta(q_0,\sqcup)=(q_{accept},\sqcup,L)$



___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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