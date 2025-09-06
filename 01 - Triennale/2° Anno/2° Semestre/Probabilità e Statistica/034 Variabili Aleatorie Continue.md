---
aliases:
  - Funzione di Densità
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: V.A. Continue
cssclasses:
  - 
---
>Una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ viene detta **Continua** se essa può assumere un valore appartenente allo [[010 Spazio Campionario|Spazio Campionario]] composto da un numero *infinito* di valori possibili. 

>Ovvero, se esiste una [[011 Funzioni|funzione]] $f:\mathbb{R}\to[0,+\infty)$ ed un intervallo $(a,b)$ di numeri reali, allora risulta che la probabilità della variabile aleatoria in questo intervallo equivale al seguente [[066 Integrali|integrale]] $$\color{#CC241D}\mathbb{P}(a\leq X\leq b)=\int_a^b f(x)dx=\displaystyle\sum_{k: a\leq x_k\leq b} p(x_k)$$

^dc25eb

![[Pasted image 20250321110340.png|350]]


Un altro modo per scrivere la definizione, utilizza un sottoinsieme $B$ di numeri reali, con $$\mathbb{P}(X\in B)=\int_B f(x)dx$$

> [!note] Probabilità per una certa $x$
> Dato che $X$ è una funzione che assume tutti i valori in un intervallo $(a,b)$, i casi possibili sono infiniti (in quanto esistono infiniti punti), quindi *non è possibile calcolare la probabilità che $X$ assuma una certa $x$*, ovvero:
> $$\mathbb{P}(X=x)=\frac{1}{\infty}=0$$


## Funzione di Densità
>Data una variabile aleatoria continua $X$ che assume valori nell'intervallo $(a,b)$, la funzione $\color{#61AFEF}f:\mathbb{R}\to[0,+\infty)$ all'interno della [[#^dc25eb|formula originale]] è chiamata **Funzione di Densità** (Probability Density Function, PDF). 
>
>Ad ogni reale associa il [[017 Limiti|limite]] per $dx$ che tende a $0$ del rapporto tra la probabilità che la VA assuma valori nell'intervallo $(x, x+dx]$ e l'ampiezza $dx$.
>$$\color{#CC241D}x\to\lim\limits_{dx \to 0}\left[ \frac{\mathbb{P}(x<X \leq x+dx)}{dx} \right]$$

La funzione di densità in $x$, allora, rappresenta quanto vale la probabilità "intorno ad x" in rapporto all'ampiezza di tale "[[009 Intorno di un Punto|intorno]]". Il termine funzione di densità, serve proprio ad evocare quanto è densa la probabilità.

![[Pasted image 20250321114316.png|500]]

### Calcolare la Funzione di Densità
>Data una [[024 Funzione di Distribuzione|funzione di distribuzione]] $F(x)$ per una variabile aleatoria continua $X$, è possibile ricavare la funzione di densità $f(x)$ **applicando la [[046 Derivata di una Funzione|derivata]] ai valori che può assumere**.


> [!example]+ <font color="orange">Esempio</font>
> Si consideri la variabile aleatoria continua X, avente funzione di distribuzione con $0\leq p \leq 1$:
> 
> $$F(x)=\begin{cases}0, & x<0 \\px, & 0\leq x < 1 \\(1-p)x + (2p-1), & 1\leq x < 2 \\1, & x\geq 2\end{cases}$$
> 
> ![[Pasted image 20250321111150.png|600]]
> 
> È possibile ricavare la funzione di densità $f(x)$ in questo modo:
> $$\begin{align}f(x)&=\frac{d}{dx}\left[ F(x) \right]\\ &=\begin{cases}\frac{d}{dx}\left[0 \right], & x<0 \\\frac{d}{dx}\left[px \right], & 0\leq x <1 \\\frac{d}{dx}\left[(1-p)x + 2p-1 \right], & 1\leq x <2 \\ \frac{d}{dx}\left[1 \right], & x\geq 2 \end{cases} \\ &=\begin{cases}p, & 0<x<1\\ 1-p, & 1\leq x < 2 \\ 0, & \text{altrimenti} \end{cases}\end{align}$$
> 
> ![[Pasted image 20250321113234.png|600]]
> 
> Dove (considerare $p$ come una costante, quindi applicare la [[048 Calcolo delle Derivate di una Funzione#Prodotto di una Costante per una Funzione|proprietà della derivata del prodotto di una costante per una funzione]]):
> 
> 1. Per via della [[047 Derivate Fondamentali#F01 Funzione Lineare e Costante 759638 Funzione Costante|derivata di una costante]] $$\frac{d}{dx}\left[0 \right]=0$$
> 2. Per via della [[047 Derivate Fondamentali#Funzione Identità|derivata di x]] $$\begin{align}\frac{d}{dx}\left[px \right] &= p\left( \frac{d}{dx}\left[x \right] \right) \\ &= p\cdot 1 \\ &=p\end{align}$$
> 3. Per via della [[048 Calcolo delle Derivate di una Funzione#Somma e Differenza tra due Funzioni|proprietà della somma e differenza tra funzioni]] $$\begin{align}\frac{d}{dx}\left[(1-p)x + 2p-1 \right] &= \frac{d}{dx}\left[(1-p)x\right] + \frac{d}{dx}\left[2p \right] - \frac{d}{dx}\left[1 \right]\\ & = (1-p)\left( \underbrace{ \frac{d}{dx}\left[x \right] }_{ 1 } \right) + 2\left( \underbrace{ \frac{d}{dx}\left[p \right] }_{ 0 } \right) - 0 \\ &= [(1-p)\cdot 1] + [2\cdot 0]\\ &= 1-p\end{align}$$
> 4. Per via della derivata di una costante $$\frac{d}{dx}\left[1 \right]=0$$


### Proprietà
$$f(x)\geq0,\ \ x\in\mathbb{R}$$
$$\mathbb{P}(-\infty\leq X \leq +\infty)=\int_{-\infty}^{\infty} f(x)dx=1$$
$$\mathbb{P}(X=a=b)=\int_a^a f(x)dx=0$$
$$F(x)=\mathbb{P}(X\leq x)=\mathbb{P}(X\in (-\infty, x]) = \int_{-\infty}^{x} f(t)dt$$
$$\mathbb{P}(a<X\leq b)=F(b)-F(a)=\int_{a}^{b} f(x)dx$$


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [[025 Variabili Aleatorie Discrete]]
- http://progettomatematica.dm.unibo.it/Prob2/6funzionedidens.html