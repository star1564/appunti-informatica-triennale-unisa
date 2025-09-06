---
aliases:
  - Scala di Infiniti
  - Forme Determinate
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---
## Scala di Infiniti
>La **Scala di Infiniti** (o Confronto tra Infiniti) è una tecnica di calcolo dei limiti che consente di stabilire, ove possibile, quale tra due funzioni divergenti diverge più velocemente all'infinito.

Si usa principalmente per risolvere la [[038 Forme Indeterminate dei Limiti|forma indeterminata]] $\frac{\infty}{\infty}$.

In alcune [[011 Funzioni|funzioni]] (o [[029 Successioni|successioni]]) capita che queste "tendono all'infinito *più velocemente*".
Si esprime questo fatto dicendo che una funzione al divergere all'infinito ha uno specifico **Ordine di Infinito** (o velocità).

> [!example]+ <font color="orange">Esempio</font>
>Confrontiamo, per $x\to+\infty$, i grafici della funzione identità $y=x$ e della [[F07 Funzione Esponenziale|funzione esponenziale]] $y = e^x$.
>
>![[Pasted image 20220401111048.png|450]]
>
>Anche se entrambe le funzioni [[032 Limiti di Successioni#Divergente|divergono]] positivamente per $x\to+\infty$ $$\lim\limits_{x \to +\infty}e^x=+\infty;\ \lim\limits_{x \to +\infty}x=+\infty$$
>dal grafico è evidente che per $x\to+\infty$ l'esponenziale (in rosso) diverge a $+\infty$ più velocemente rispetto alla funzione identità.

### Gerarchia degli Infiniti
La *Gerarchia degli Infiniti* cui si fa riferimento è ottenuta per $x\to+\infty$ (dove $a << b$ significa che $a$ è più veloce di $b$): $$log_ax<<x^a<<a^x<<x!<<x^x$$

## Calcolo degli Infiniti (Forme Determinate)

Non tutte le volte che si trova lo zero o infinito in un limite si tratta di una [[038 Forme Indeterminate dei Limiti|forma indeterminata]].

Infatti queste sono le **Forme Determinate**, che stabiliscono i risultati delle operazioni contenenti gli infiniti:
- $k+\infty=+\infty$
- $k-\infty=-\infty$
- $\infty+\infty=\infty$
- $k\cdot \infty=\infty$
- $\infty\cdot \infty=\infty$
- $\frac{k}{\infty}=0$
- $\frac{0}{\infty}=0$
- $\frac{\infty}{k}=\infty$
- $\frac{k}{0}=\infty$




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
