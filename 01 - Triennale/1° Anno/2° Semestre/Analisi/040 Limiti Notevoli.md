---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---
>I **Limiti Notevoli** sono particolari [[037 Limiti di Funzioni|limiti di funzioni]] elementari che vengono utilizzati nel calcolo dei limiti più complessi e delle [[038 Forme Indeterminate dei Limiti|forme indeterminate]]. La $x$ si può sostituire con una funzione $f(x)$.


|                                       Limite                                       |      Forma Indeterminata       |                                                                                                                                 Ottenuta da                                                                                                                                 |
| :--------------------------------------------------------------------------------: | :----------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|            $$\color{#FF6611}\lim\limits_{x \to 0}\frac{\sin(x)}{x}=1$$             | $$\left[ \frac{0}{0} \right]$$ |                                                                                                              <i><font color="#FF6611">Fondamentale</font></i>                                                                                                               |
|   $$\color{#00C575}\lim\limits_{x \to \pm\infty}\left(1+\frac{1}{x}\right)^x=e$$   |  $$\left[ 1^\infty \right]$$   |                                                                                                              <i><font color="#00C575">Fondamentale</font></i>                                                                                                               |
|                    $$\lim\limits_{x \to 0}\frac{\tan(x)}{x}=1$$                    | $$\left[ \frac{0}{0} \right]$$ |                                                                                       $\lim\limits_{x \to 0} \textcolor{#FF6611}{\frac{\sin(x)}{x}}\cdot \frac{1}{\cos(x)}=1\cdot1=1$                                                                                       |
|          $$\lim\limits_{x \to 0}\frac{1-\cos(x)}{x^2}=\frac{1}{2}$$ [^2]           | $$\left[ \frac{0}{0} \right]$$ | $\lim\limits_{x \to 0} \frac{1-\cos(x)}{x^2}\cdot \frac{1+\cos(x)}{1+\cos(x)}=\lim\limits_{x \to 0} \frac{1-\cos^2}{x^2(1+\cos(x))}=$ [^1]<br>$=\lim\limits_{x \to 0} \textcolor{#FF6611}{\frac{\sin^2(x)}{x^2}}\cdot \frac{1}{1+\cos(x)}=1^2\cdot \frac{1}{2}=\frac{1}{2}$ |
|            $$\color{#e86162}\lim\limits_{x \to 0}\frac{\ln(1+x)}{x}=1$$            | $$\left[ \frac{0}{0} \right]$$ |                                                  $\lim\limits_{x \to 0}\ln(1+x)^{1/x}=\left[ y=\frac{1}{x} \right]= \lim\limits_{y \to \infty}\ln\textcolor{#00C575}{\left( 1+\frac{1}{y} \right)^{y}}=$<br>$=\ln (e) = 1$                                                  |
|                  $$\lim\limits_{x \to 0}\frac{e^x-1}{x}=1$$ [^2]                   | $$\left[ \frac{0}{0} \right]$$ |                                                             $\color{#61AFEF}e^x-1=y \iff e^x=1+y \iff x=\ln(1+y)$<br>$\lim\limits_{y \to 0} \textcolor{#e86162}{\left(\frac{y}{\ln(1+y)}\right)}=\frac{1}{1}=1$                                                             |
|             $$\lim\limits_{x \to 0}\frac{a^x-1}{x}=\ln(a),\quad a>0$$              | $$\left[ \frac{0}{0} \right]$$ |                                                                                                                          $$\color{#e86162}\star$$                                                                                                                           |
| $$\lim\limits_{x \to 0}\frac{\log_a(1+x)}{x}=\frac{1}{\ln(a)},\quad a>0,\ a\neq1$$ | $$\left[ \frac{0}{0} \right]$$ |                                                                                                                          $$\color{#e86162}\star$$                                                                                                                           |
|                  $$\lim\limits_{x \to 0}\frac{\arcsin(x)}{x}=1$$                   | $$\left[ \frac{0}{0} \right]$$ |                                                                                                                                  $$\dots$$                                                                                                                                  |
|                  $$\lim\limits_{x \to 0}\frac{\arctan(x)}{x}=1$$                   | $$\left[ \frac{0}{0} \right]$$ |                                                                                                                                  $$\dots$$                                                                                                                                  |
|      $$\lim\limits_{x \to 0}\frac{(1+x)^c-1}{x}=c,\quad\quad c\in\mathbb{R}$$      | $$\left[ \frac{0}{0} \right]$$ |                                                                                                                                  $$\dots$$                                                                                                                                  |
|     $$\lim\limits_{x \to \pm\infty}\frac{1}{x^p}=0,\quad\quad p\in\mathbb{N}$$     |  $$\left[ 1^\infty \right]$$   |                                                                                                                                  $$\dots$$                                                                                                                                  |

[^1]: [[019 Definizione di Seno e Coseno#^37a9af|Relazione fondamentale della goniometria]]
[^2]: [[041 Calcolo dei Limiti di una Funzione#Limite Notevole Reciproco|Limite Notevole Reciproco]]


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

- [Lista di Limiti Notevoli su YouMath](https://www.youmath.it/lezioni/analisi-matematica/limiti-continuita-e-asintoti/136-limiti-notevoli-quali-sono.html)
- [Video Elia Bombardelli con Esercizi di esempio](https://www.youtube.com/watch?v=ejmts9Qsz8Q&list=PLpkXLf6Zhdx00lP1ul6ez4F5C_VCCRjos&index=9)