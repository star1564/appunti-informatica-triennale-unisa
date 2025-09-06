---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>La **Formula di Integrazione per Parti** consente di integrare facilmente tutte quelle funzioni che si presentano nella forma di *prodotto* di una prima funzione $f(x)$ per la [[046 Derivata di una Funzione|derivata]] di una seconda funzione $g'(x)$. 
>
>Questo lo fa attraverso la semplificazione dell'integrale:
>$$\textcolor{#61AFEF}{\int f(x)\cdot g'(x)\, dx} = \color{#CC241D}f(x)\cdot g(x)\ -\ \int f'(x)\cdot g(x)\, dx$$



> [!example]+ <font color="orange">Esempio 1</font>
> Calcolare il seguente integrale.
> > $$\int x\cdot\cos(x)\, dx$$
> 
> Poniamo:
> - $f(x)=\textcolor{#61AFEF}x,\quad f'(x)=1$
> - $g'(x)=\textcolor{#61AFEF}{\cos(x)},\quad g(x)=\sin(x)$
>
> Quindi, dove $c\in \mathbb{R}$:
> $$\begin{align*}\int x\cdot\cos(x)\, dx &= x\cdot \sin(x)-\int \frac{d}{dx}\left[ x\right]\cdot \sin(x)\, dx \\ &= x\cdot \sin(x)-\int 1\cdot \sin(x)\, dx \\ &=x\cdot \sin(x)-\int \sin(x)\, dx \\ &= x\cdot \sin(x) - (- \cos(x) + c) \\ &= x\cdot \sin(x) + \cos(x) + c\end{align*}$$



> [!example]+ <font color="orange">Esempio 2</font>
> Calcolare il seguente integrale.
> > $$\int x\cdot e^x\ dx$$
> 
> Poniamo:
> - $f(x)=\textcolor{#61AFEF}x,\quad f'(x)=1$
> - $g'(x)=\textcolor{#61AFEF}{e^x},\quad g(x)=e^x$
>
> Quindi, dove $c\in \mathbb{R}$:
> $$\begin{align*}\int x\cdot e^x\, dx &= x\cdot e^x-\int \frac{d}{dx}\left[ x\right]\cdot e^x\, dx \\ &= x\cdot e^x-\int 1\cdot e^x\, dx \\ &= x\cdot e^x-\int e^x\, dx \\ &=x\cdot e^x-e^x+c\end{align*}$$

> [!example]+ <font color="orange">Esempio 3</font>
> Calcolare il seguente integrale.
> > $$\int (x^4+3x^2-6)e^x\ dx$$
>
> Andando a semplificare la funzione ed utilizzando una delle [[070 Proprietà degli Integrali#Addizione e Sottrazione di Integrali|proprietà per le addizioni negli integrali]] lo trasformiamo in questo modo:
>
>$$\begin{align*}\int (x^4+3x^2-6)e^x\, dx &= \int x^4e^x\, dx + \int 3x^2e^x\, dx + \int (-6)e^x\, dx \\ &=\textcolor{#00C575}{\int x^4e^x\, dx}+\textcolor{#00C575}{3\int x^2e^x\, dx}-\textcolor{#FF6611}{6\int e^x\, dx}\\ &= \dots \end{align*}$$
>
>I <font color="#00C575">primi due</font> si possono risolvere con la formula principale dell'integrazione per parti, in cui viene usata più di una volta.
>Mentre l'<font color="#FF6611">ultimo</font> è immediato.



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/integrali/558-come-integrare-per-parti.html)
- [Video Elia Bombardelli](https://www.youtube.com/watch?v=2D2-g93Kljo)
