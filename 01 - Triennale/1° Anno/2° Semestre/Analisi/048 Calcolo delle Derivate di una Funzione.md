---
aliases:
  - Derivata di una Funzione Composta
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
>Il **Calcolo delle [[046 Derivata di una Funzione|Derivate]]** è un processo che si basa su un insieme di regole, dette *regole di derivazione*.

Sono usate per evitare di avere calcoli enormi con la formula del [[045 Rapporto Incrementale|Rapporto Incrementale]].

Si ricorda che $\frac{d}{dx}$ è un sinonimo di $f'(x)$.

### Prodotto di una Costante per una Funzione
È uguale al prodotto della costante per la derivata della funzione: 
$$\color{#61AFEF}\frac{d}{dx}[c\cdot f(x)]=c\cdot\frac{d}{dx}f(x)$$

### Somma e Differenza tra due Funzioni

È uguale alla somma (o differenza) delle singole derivate. 
$$\color{#61AFEF}\frac{d}{dx}[f(x)\pm g(x)]=\frac{d}{dx}f(x)\pm\frac{d}{dx}g(x)$$

### Prodotto di due Funzioni
È data dalla somma tra il prodotto della prima funzione derivata per la seconda non derivata, e la prima funzione non derivata per la seconda derivata. 
$$\color{#61AFEF}\frac{d}{dx}[f(x)\cdot g(x)]=\frac{d}{dx}\left[f(x)\right]\cdot g(x)+f(x)\cdot\frac{d}{dx}\left[g(x)\right]$$

### Rapporto di due Funzioni

$$\color{#61AFEF}\frac{d}{dx}\left(\frac{f(x)}{g(x)}\right) = \frac{\frac{d}{dx}[f(x)]\cdot g(x)-f(x)\cdot\frac{d}{dx}[g(x)]}{g^2(x)}$$

### Reciproco di una Funzione
Sia $f(x)$ una [[046 Derivata di una Funzione#Funzione Derivabile|funzione derivabile]], la *derivata del reciproco di una funzione* è: 
$$\color{#61AFEF}\frac{d}{dx}\left[\frac{1}{f(x)}\right]=\frac{-(\frac{d}{dx}[f(x)])}{[f(x)]^2}$$

## Teorema per la Derivata della Funzione Composta
Considerare due funzioni reali di variabile reale $f:\mathbb{R}\to\mathbb{R}$, $g:\mathbb{R}\to\mathbb{R}$ e la loro [[023 Applicazioni Composte|funzione composta]] $g(f(x))$. 

Vale allora che:
$$\color{#61AFEF}\frac{d}{dx}[g(f(x))]=g'(f(x))\cdot f'(x)$$


> [!example]+ <font color="orange">Esempio</font>
>$$
\begin{align*}
\frac{d}{dx}[\ln(\cos(x))] &= \frac{d}{dx}[\ln(\cos(x))]\cdot \frac{d}{dx}[\cos(x)] \\
&= \frac{1}{\cos(x)}\cdot (-\sin(x)) \\
&= -\left(\frac{\sin(x)}{\cos(x)}\right) \\
&= -\tan(x)
\end{align*}
>$$

### Derivata di una Funzione Elevata ad un Numero
Come conseguenza della derivata della funzione composta, è anche vero che:
$$\color{#61AFEF}\frac{d}{dx}\left[f(x)^n \right]=n\cdot f(x)\cdot \frac{d}{dx}\left[f(x)\right]$$


> [!example]+ <font color="orange">Esempio</font>
>$$
\begin{align*}
\frac{d}{dx}\left[(x-1)^3 \right]&=3\cdot (x-1)^2\cdot \frac{d}{dx}\left[x-1 \right] \\
&= 3\cdot(x-1)^2\cdot 1 \\
&= 3(x^2 - 2 x + 1) \\
&= 3x^2 - 6 x + 3
\end{align*}
>$$

### Derivata della Radice di una Funzione
Come conseguenza della derivata della funzione composta, è anche vero che:
$$\color{#61AFEF}\frac{d}{dx}\left[\sqrt[]{f(x)} \right]=\frac{1}{2\sqrt[]{f(x)}}\cdot \frac{d}{dx}\left[f(x)\right]$$


## Derivata di $f(x)$ elevato a $g(x)$
$$\frac{d}{dx}[f(x)]^{[g(x)]}$$

Si deve usare la strategia seguente, dove: 
- $f(x)=a$
- $g(x)=b$

$$\frac{d}{dx}[a^b]=\frac{d}{dx}\left[e^{\ln(a^b)}\right]=\frac{d}{dx}\left[e^{b\cdot \ln(a)}\right]$$
Poi bisogna utilizzare il teorema per [[#Teorema per la Derivata della Funzione Composta|derivare una funzione composta]].

> [!example]+ <font color="orange">Esempio</font>
>Calcolare $$y'=\frac{d}{dx}[x^x]$$
>
>$$
\begin{align*}
\frac{d}{dx}[x^x] &= \frac{d}{dx}[e^{\ln(x^x)}] \\
&= \frac{d}{dx}[\textcolor{#CC241D}e^\textcolor{#61AFEF}{{x\cdot \ln(x)}}] = [\text{derivazione della composta}] \\
&= \textcolor{#CC241D}{\frac{d}{dx}[e^{x\cdot \ln(x)}]}\cdot \textcolor{#61AFEF}{\frac{d}{dx}[x\cdot \ln(x)]} \\
&= \textcolor{#CC241D}{e^{x\cdot \ln(x)}}\cdot \textcolor{#61AFEF}{\frac{d}{dx}[x\cdot \ln(x)]} = [\text{derivazione del prodotto}] \\
&= \textcolor{#CC241D}{e^{x\cdot \ln(x)}}\cdot \textcolor{#61AFEF}{\left[\frac{d}{dx}[x]\cdot \ln(x)+x\cdot \frac{d}{dx}[\ln(x)]\right]} \\
&= \textcolor{#CC241D}{e^{x\cdot \ln(x)}}\cdot \textcolor{#61AFEF}{\left[1\cdot \ln(x)+x\cdot \frac{1}{x}\right]} \\
&= \textcolor{#CC241D}{e^{x\cdot \ln(x)}}\cdot \textcolor{#61AFEF}{[\ln(x)+1]}= [\text{dato che } x^x\equiv e^{x\cdot \ln(x)}] \\
&= \textcolor{#CC241D}{x^x}\textcolor{#61AFEF}{[\ln(x)+1]} \\
&=y'
\end{align*}
>$$



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/derivate/213-algebra-delle-derivate-regole-di-derivazione.html)
- [Video 1 Elia Bombardelli con Dimostrazioni (Somma/Differenza e Prodotto)](https://www.youtube.com/watch?v=4dwPWLPivgQ&list=PLpkXLf6Zhdx3cfwlkGty-daBIHvPj4OuW&index=4)
- [Video 2 Elia Bombardelli con Dimostrazioni (Composta)](https://www.youtube.com/watch?v=cy4jWe2UvbU&list=PLpkXLf6Zhdx3cfwlkGty-daBIHvPj4OuW&index=6)
- [Video 3 Elia Bombardelli con Esercizi (Derivata di f(x)^g(x))](https://www.youtube.com/watch?v=K2PoVYDI0IY&list=PLpkXLf6Zhdx3cfwlkGty-daBIHvPj4OuW&index=8)