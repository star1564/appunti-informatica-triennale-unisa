---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---
>Lo **Studio di una [[011 Funzioni|Funzione]]** è un procedimento analitico che consiste di vari passaggi e che permette, partendo dal dominio e arrivando allo studio della derivata seconda, di tracciare il [[012 Grafici di Funzioni|grafico]] qualitativo *analizzando l'espressione della funzione*.

Si considera come <font color="orange">esempio caso studio</font> sempre la seguente funzione: $$y=\frac{\ln(x+1)}{-2-\ln(x+1)}$$
I passaggi da svolgere per studiare una funzione sono i seguenti, (bisogna ricordarsi di aggiornare il grafico della funzione ad ogni passo):
1. [[059 Studio del Dominio di una Funzione|Calcolare il Dominio della Funzione]]
2. [[060 Studio della Parità e Disparità di una Funzione|Vedere se la Funzione è Pari o Dispari]]
3. [[061 Studio dell_Intersezione degli assi di una Funzione|Vedere le Intersezioni con gli Assi x e y del Grafico]]
4. [[062 Studio del Segno di una Funzione|Vedere il segno della funzione]]
5. [[063 Studio degli Asintoti e dei Limiti agli estremi del dominio|Vedere se ci sono asintoti verticali, orizzontali o obliqui]]
6. [[064 Studio della Derivata Prima|Vedere il segno della derivata prima]]
7. [[065 Studio della Derivata Seconda|Trovare i punti di flesso e studiare il segno della derivata seconda]]
8. Disegnare il grafico

Il grafico della funzione caso di studio (che si ricava alla fine) è il seguente:

![[Pasted image 20220505121014.png|800]]

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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


- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico.html)
- [Video Elia Bombardelli](https://www.youtube.com/watch?v=4qglsj_Br5w)
- Esempi ed esercizi
	- [Esempio Studio Funzione (YouMath)](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/3371-esempio-studio-di-funzione.html)
	- [Esercizi Studio di Funzione (YouMath)](https://www.youmath.it/esercizi/es-analisi-matematica/es-sullo-studio-di-funzioni.html)
- Per i grafici:
	- [Desmos (Calcolatrice Grafica)](https://www.desmos.com/calculator?lang=it) 
	- [Geogebra (Calcolatrice Grafica)](https://www.geogebra.org/classic)