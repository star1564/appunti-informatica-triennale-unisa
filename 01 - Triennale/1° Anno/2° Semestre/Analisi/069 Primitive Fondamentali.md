---
aliases:
  - Primitive Elementari
  - Primitive Elementari Generalizzate
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Una **Primitiva Elementare** (o fondamentale) è praticamente l'*opposto* della [[047 Derivate Fondamentali|derivata fondamentale]].


> [!note]+ Non tutti gli integrali sono facilmente riconducibili a primitive elementari
> Per quelli (e **anche quelli riconducibili a primitive elementari**) si utilizzano altre tecniche di integrazione, come:
> - [[076 Integrazione per Parti|l'integrazione per parti]]
> - [[073 Integrazione per Sostituzione|l'integrazione per sostituzione]]


Si hanno le seguenti primitive, con $c\in\mathbb{R}$:

$$\int \textcolor{#61AFEF}{f'(x)}\, dx=\textcolor{#CC241D}{f(x)+c}$$

$$\int \textcolor{#61AFEF}{k}\, dx=\textcolor{#CC241D}{k\cdot x+c}$$

$$\int \textcolor{#61AFEF}{x^n} dx=\textcolor{#CC241D}{\frac{x^{n+1}}{n+1}+c}, \quad \forall n\neq-1$$ ^34736c

$$\int \textcolor{#61AFEF}{x} \, dx=\textcolor{#CC241D}{\frac{x^{2}}{2}+c}$$

$$\int \textcolor{#61AFEF}{\cos(x)} \, dx=\textcolor{#CC241D}{\sin(x)+c}$$


$$\int \textcolor{#61AFEF}{\sin(x)} \, dx=\textcolor{#CC241D}{-\cos(x)+c}$$


$$\int \textcolor{#61AFEF}{e^x} \, dx=\textcolor{#CC241D}{e^x+c}$$

$$\int \textcolor{#61AFEF}{a^x} \, dx=\textcolor{#CC241D}{\frac{a^x}{\ln(a)}+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{x}} \, dx=\textcolor{#CC241D}{\ln(|x|)+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{1+x^2}} \, dx=\textcolor{#CC241D}{\arctan(x)+c}$$

$$\int \textcolor{#61AFEF}{\sqrt{x}} \, dx=\textcolor{#CC241D}{\frac{2}{3}\cdot x^{\frac{3}{2}}+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{\cos^2(x)}} \, dx=\textcolor{#CC241D}{\tan(x)+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{\sin^2(x)}} \, dx=\textcolor{#CC241D}{-\cot(x)+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{\sqrt{1-x^2}}} \, dx=\textcolor{#CC241D}{\arcsin(x)+c}$$



> [!example]- <font color="orange">Esempi</font>
> $$\int_0^\frac{\pi}{2} \sin(x)dx$$
>
>
>$$\int_0^\frac{\pi}{2} \sin(x)dx=[-\cos(x)+c]_0^\frac{\pi}{2}=\left(-\cos\left(\frac{\pi}{2}\right)+c\right)-(-\cos(0)+c)=(0+c)+(1-c)=\textcolor{orange}1$$
>
>![[Pasted image 20220507120431.png]]
>
>---
>$$\int_1^2 x^3dx$$
>
>$$F(x)=\int x^3= \frac{x^4}{4}+c,\quad c\in\mathbb{R}$$
>
>$$\int_1^2 x^3dx=\left[\frac{x^4}{4}+c\right]_1^2=\left(\frac{2^4}{4}+c\right)-\left(\frac{1^4}{4}+c\right)=\left(\frac{16}{4}+c\right)+\left(-\frac{1}{4}-c\right)=\frac{15}{4}$$

## Primitive Elementari Generalizzate
Le primitive elementari possono anche essere **generalizzate** quando l'argomento è una generica $\textcolor{#fdaa39}{f(x)}$ a patto che nell'integrale sia moltiplicata alla sua derivata, ovvero $\textcolor{#00C575}{f^{\prime}(x)}$.

Alcune di queste sono:


$$\int\cos\left(\textcolor{#fdaa39}{f(x)}\right)\cdot \textcolor{#00C575}{f^{\prime}(x)}\, dx=\sin\left(\textcolor{#fdaa39}{f(x)}\right)+c$$

$$\int\sin\left(\textcolor{#fdaa39}{f(x)}\right)\cdot \textcolor{#00C575}{f^{\prime}(x)}\, dx=-\cos\left(\textcolor{#fdaa39}{f(x)}\right)+c$$

$$\int e^{\textcolor{#fdaa39}{f(x)}}\cdot \textcolor{#00C575}{f^{\prime}(x)}\, dx=e^{\textcolor{#fdaa39}{f(x)}}+c$$

$$\int\left[\textcolor{#fdaa39}{f(x)}\right]^{n}\cdot \textcolor{#00C575}{f^{\prime}(x)}\, dx={\frac{\left[\textcolor{#fdaa39}{f(x)}\right]^{n+1}}{n+1}}+c$$
$$\int \frac{\textcolor{#00C575}{f^{\prime}(x)}}{\textcolor{#fdaa39}{f(x)}}\, dx= \ln(\left|\textcolor{#fdaa39}{f(x)} \right|)+c$$

$$\int \frac{\textcolor{#00C575}{f^{\prime}(x)}}{1+[\textcolor{#fdaa39}{f(x)}]^2}\, dx= \text{arctg}(\textcolor{#fdaa39}{f(x)} )+c$$ ^0570b7

> [!example]+ <font color="orange">Esempio</font>
>$$\int \frac{\textcolor{#00C575}{\cos(x)}}{\textcolor{#fdaa39}{\sin(x)}}\, dx= \ln(\left|\textcolor{#fdaa39}{\sin(x)} \right|)+c$$
>
>Questo perché $$f'(\sin(x))=\cos(x)$$


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
- [YouMath (Primitive Fondamentali)](https://www.youmath.it/lezioni/analisi-matematica/integrali/596-integrali-notevoli.html)
- [Elia Bombardelli](https://youtu.be/4hfhVhnzuUw?list=PLD65828BD6F3E86AA&t=178)