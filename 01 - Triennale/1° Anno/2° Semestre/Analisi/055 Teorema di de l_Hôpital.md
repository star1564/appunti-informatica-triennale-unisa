---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
>Il **Teorema di De L'Hôpital** è un teorema sui limiti di funzioni reali di variabile reale che, sotto le opportune ipotesi, consente di [[041 Calcolo dei Limiti di una Funzione|calcolare il limite]] di un *rapporto di funzioni* quando da questo si ottiene una delle due seguenti [[038 Forme Indeterminate dei Limiti|forme indeterminate]]:
>$$\Big[\frac{0}{0}\Big],\quad \Big[\frac{\infty}{\infty}\Big]$$


> [!quote] <font color="orange">Teorema di De L'Hôpital</font>
> Sia $x_0\in(a,b)$ e siano $f,g:(a,b)\to \mathbb{R}$ due funzioni, tali che:
> 1. $f(x_0)=0$
> 2. $g(x_0)=0$
> 3. $f$ e $g$ sono [[035 Funzione Continua|continue]] in $(a,b)$ e [[046 Derivata di una Funzione#Funzione Derivabile|derivabili]] in $(a,b)-\left\{ x_0 \right\}$
> 4. $g'(x)\neq 0,\quad \forall x\in (a,b)-\left\{ x_0 \right\}$
> 5. Esiste (finito o infinito) il [[037 Limiti di Funzioni|limite]] $\lim\limits_{x \to x_0} \frac{f'(x)}{g'(x)}$ 
> 
> Allora esiste $\lim\limits_{x \to x_0} \frac{f(x)}{g(x)}$ e vale che
> $$\color{#CC241D}\lim\limits_{x \to x_0} \frac{f(x)}{g(x)} = \lim\limits_{x \to x_0} \frac{f'(x)}{g'(x)}$$



> [!success]- Dimostrazione
> Sia $x\in(a,b)$ tale che $\color{#61AFEF}x>x_0$.
> Per il [[053 Teorema di Cauchy|teorema di Cauchy]] esiste un punto $c(x)\in (x_0,x)$ tale che:
> $$\frac{f(x)-f(x_0)}{g(x)-g(x_0)}=\frac{f'(c(x))}{g'(c(x))}$$
> 
> Questa applicazione del teorema di Cauchy è possibile per il punto 4 della definizione del teorema di De L'Hôpital.
> 
> ![[Pasted image 20241003180758.png|600]]
> 
> Per via del punto 1 e 2, si ha che:
> 
> $$\lim\limits_{x \to x_0^+}\frac{f(x)}{g(x)}= \lim\limits_{x \to x_0^+}\frac{f'(c(x))}{g'(c(x))}$$
> 
> Ma vale che: 
> $$\lim\limits_{x \to x_0^+} c(x)=x_0$$
> 
> ![[Pasted image 20241003182602.png|600]]
> 
> Quindi:
> $$\color{#FF6611}\lim\limits_{x \to x_0^+}\frac{f(x)}{g(x)}= \lim\limits_{x \to x_0^+}\frac{f'(c(x))}{g'(c(x))}=\lim\limits_{x \to x_0^+} \frac{f'(x)}{g'(x)}$$
> 
> Analogamente per $\color{#61AFEF}x<x_0$:
> $$\color{#FF6611}\lim\limits_{x \to x_0^-}\frac{f(x)}{g(x)}= \lim\limits_{x \to x_0^-}\frac{f'(c(x))}{g'(c(x))}=\lim\limits_{x \to x_0^-} \frac{f'(x)}{g'(x)}$$
> 
> 
> Pertanto, si ha la relazione nella definizione:
> $$\color{#CC241D}\lim\limits_{x \to x_0} \frac{f(x)}{g(x)} = \lim\limits_{x \to x_0} \frac{f'(x)}{g'(x)}$$


> [!example]- <font color="orange">Esempio</font>
> $$\lim\limits_{x \to \frac{\pi}{2}}\frac{1-\sin(x)}{\cos(x)}$$
> Qui si può ricorrere al teorema, infatti la forma indeterminata quando si sostituisce $\frac{\pi}{2}$ alle $x$ è $[\frac{0}{0}]$.
> 
> 
> $$
> \begin{align*}
> \lim\limits_{x \to \frac{\pi}{2}}\frac{1-\sin(x)}{\cos(x)} &= \lim\limits_{x \to \frac{\pi}{2}}\frac{\frac{d}{dx}[1-\sin(x)]}{\frac{d}{dx}[\cos(x)]} \\
> &= \lim\limits_{x \to \frac{\pi}{2}}\frac{\frac{d}{dx}[1]-\frac{d}{dx}[\sin(x)]}{-\sin(x)} \\
> &= \lim\limits_{x \to \frac{\pi}{2}}\frac{0-(\cos(x))}{-\sin(x)} \\
> &= \lim\limits_{x \to \frac{\pi}{2}}\frac{-\cos(x)}{-\sin(x)} \\
> &= \frac{0}{1}=0
> \end{align*}
> $$
> 
> ![[Pasted image 20241003182423.png|700]]
> 
> ---
> 
> $$\lim\limits_{x \to +\infty}(1+\frac{1}{2x})^{3x^2}$$
> 
> Qui non si può applicare il teorema, perché andando a sostituire $+\infty$ a tutte le $x$ si ottiene la forma indeterminata $[1^\infty]$.

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
- [Francesco Bigolin](https://youtu.be/qpLQodww7I4?si=QErUnRpRc0Ix54w7)
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/limiti-continuita-e-asintoti/205-il-teorema-di-de-lhopital.html)
- https://youtu.be/kfF40MiS7zA?t=593