---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---
> [!info] Nota (3/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
> [[060 Studio della Parità e Disparità di una Funzione|<- Passo precedente]] | [[062 Studio del Segno di una Funzione|Prossimo passo ->]]

>Con l'espressione **Intersezioni con gli Assi** si indicano le intersezioni del grafico di una funzione $y=f(x)$ con gli [[012 Grafici di Funzioni|assi cartesiani]].

### Intersezione con l'asse $y$
Se il seguente è vero, allora il grafico della funzione interseca l'asse delle $y$:
$$\color{#61AFEF}x=0\in Dom(f)$$
In caso contrario, se $x=0\not\in Dom(f)$, allora non è presente alcuna intersezione con l'asse delle [[012 Grafici di Funzioni#^69bcb6|ordinate]].

Per calcolare l'*ordinata* del punto di intersezione con l'asse $y$ bisogna semplicemente *valutare la funzione in corrispondenza dell'[[012 Grafici di Funzioni#^69bcb6|ascissa]] $x=0$*, ossia sostituire il valore di $x$ con $0$ nell'espressione della funzione.
$$\text{Coordinate intersezione asse }y:(0,\ f(0))$$

### Intersezione con l'asse $x$
In generale, il grafico di una funzione può intersecare o non l'asse delle $x$ e, *se lo interseca, lo può fare in uno o più punti*.

Per calcolare le coordinate dei punti di intersezione con l'asse $x$, di equazione $y=0$, bisogna vedere quali sono i valori di $x$ per i quali la funzione si annulla.

Bisogna quindi risolvere l'equazione
$$\text{Coordinate intersezioni asse }x: f(x)=0$$

> [!example] <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{ln(x+1)}{-2-ln(x+1)}$$
> - *Intersezione con l'asse delle $y$*:
>   Vediamo prima di tutto che $x=0\in Dom(f)$, quindi calcoliamo la funzione quando $x=0$.
>   $$y=f(0)=\frac{ln(0+1)}{-2-ln(0+1)}=\frac{ln(1)}{-2-ln(1)}=\frac{0}{-2}=0$$
>   La funzione interseca l'asse delle $y$ nel punto $(x,y)=(0, 0)$.
> 
> - *Intersezione con l'asse delle $x$*:
>   Ricaviamo le ascisse dei punti di intersezione risolvendo l'equazione $f(x)=0$.
>   $$\frac{ln(x+1)}{-2-ln(x+1)}=0\iff ln(x+1)=0\iff x+1=e^0\iff x+1=1\iff x=0$$
>   La funzione interseca l'asse delle $x$ nel punto $(x,y)=(0, 0)$.
> 
> Quindi la funzione interseca l'asse delle $x$ e delle $y$ nel punto $\color{orange}(0, 0)$.


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/179-step-3-intersezioni-con-gli-assi.html)