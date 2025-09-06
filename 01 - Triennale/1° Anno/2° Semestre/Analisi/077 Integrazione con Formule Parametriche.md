---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
Sia $f(x)$ una funzione con $\sin(x), \cos(x), \tan(x)$, è possibile risolvere un integrale di questa funzione attraverso le **Formule Parametriche**. 

>Ponendo attraverso la [[073 Integrazione per Sostituzione|sostituzione]]: 
>$$\color{#00C575}\tan\left(\frac{x}{2}\right)=t$$
>Si hanno le seguenti formule:
>$$\color{#CC241D}\sin(x)=\frac{2\textcolor{#00C575}t}{1+\textcolor{#00C575}t^2};\quad \cos(x)=\frac{1-\textcolor{#00C575}t^2}{1+\textcolor{#00C575}t^2};\quad \tan(x)=\frac{2\textcolor{#00C575}t}{1-\textcolor{#00C575}t^2}$$
>
>Che potremmo usare per sostituire i corrispondenti $\sin(x), \cos(x),\tan(x)$ nell'integrale.
>
>Si ha inoltre, che: 
>
>$$\begin{align*} \color{#00C575}\tan\left( \frac{x}{2} \right) &= \color{#00C575}t \\ \frac{x}{2}&= \arctan(t) \\ x&= 2\arctan(t) \\ \color{#61AFEF}dx&=\color{#61AFEF}\frac{2}{1+t^2}\,dt\end{align*}$$

> [!example]+ <font color="orange">Esempio</font>
>$$\begin{align*} \int \frac{1}{\sin(x)}\, dx &= \int \frac{1}{\color{#CC241D}\frac{2t}{1+t^2}}\cdot \textcolor{#61AFEF}{\frac{2}{1+t^2}\,dt} \\ &=\int \frac{1+t^2}{2t}\cdot\frac{2}{1+t^2}\, dt \\ &=\int \frac{2}{2t}\, dt \\ &=\int \frac{1}{t}\, dt \\ &=\ln|t| \\ &= [\text{sostituire per }x] \\ &=\ln\left\vert\textcolor{#00C575}{\tan\left( \frac{x}{2} \right)}\right\vert\end{align*}$$


> [!example]+ <font color="orange">Esempio</font>
> $$\begin{align*}\int \frac{1}{\cos(x)} &= \int \frac{1}{\color{#CC241D}\frac{1-t^2}{1+t^2}}\cdot \textcolor{#00C575}{\frac{2}{1+t^2}\, dt}\\&=\int \frac{1+t^2}{1-t^2}\cdot \frac{2}{1+t^2}\, dt  \\&= \int \frac{2}{1-t^2}\, dt \\&= 2\int \frac{1}{(1-t)(1+t)}\, dt \\&=\star\end{align*}$$
> 
> Utilizzare l'[[075 Integrazione di Razionali Fratti|integrazione di razionali fratti]].
> 
> $$\frac{A}{1-t}+\frac{B}{1+t}= \frac{t(A-B)+A+B}{(1-t)(1+t)}\quad \begin{cases} A-B=0 \\ A+B=1 \end{cases}\quad \begin{cases} A=\frac{1}{2} \\ B=\frac{1}{2} \end{cases}$$
> 
> $$\begin{align*}\star &= 2\int\left(\frac{\frac{1}{2}}{1-t} + \frac{\frac{1}{2}}{1+t}\right)\,dt \\&= 2\int \frac{1}{2} \left(\frac{1}{1-t} + \frac{1}{1+t}\right)\,dt \\&= \int \left(\frac{1}{1-t} + \frac{1}{1+t}\right)\,dt \\&= \int \frac{1}{1-t}\,dt + \int\frac{1}{1+t}\,dt \\&= \star\star\end{align*}$$
> 
> Dato che $\frac{d}{dx}\left[ 1-t\right]=-1$ e che $\frac{d}{dx}\left[1+t \right]=1$:
> 
> $$\begin{align*}\star\star &= -\int \frac{-1}{1-t}\,dt + \int\frac{1}{1+t}\,dt \\&= [\text{usare gli integrali immediati}] \\&= -\ln|1-t|+\ln|1+t| \\&= [\text{sostituire per }x] \\&= -\ln\left\vert1-\tan\left( \frac{x}{2} \right) \right\vert+\ln\left\vert1+\tan\left( \frac{x}{2} \right) \right\vert \\\end{align*}$$


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

