---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Somme di V.A.
cssclasses:
  - 
---
>Le **somme di variabili aleatorie** sono un concetto utilizzato per comprendere come si comportano la loro [[024 Funzione di Distribuzione|distribuzione]], il [[026 Valore Atteso di una VA Discreta|valore atteso]] e la [[027 Varianza di una VA Discreta|varianza]]. Ad esempio $$\color{#CC241D}Y=X_{1}+X_{2}+\dots+X_{n}$$

## Somme di VA Discrete
>Consideriamo due variabili aleatorie $X$ e $Y$. Queste devono essere: 
>- [[041 Variabili Aleatorie Indipendenti|indipendenti]]
>- [[025 Variabili Aleatorie Discrete|discrete]]
>- devono avere una [[025 Variabili Aleatorie Discrete#Densità Discreta|densità discreta]] nota
>
>Supponiamo che si voglia calcolare 
>$$\color{#CC241D}Z=X+Y$$

Allora si imposta una funzione del genere $$\color{#61AFEF}g(X,Y)=X+Y$$
La probabilità dell'evento $p_{Z}(z)$ sarà uguale alla *somma delle probabilità di tutti i possibili modi in cui questo evento può accadere*.

> [!example]+ <font color="orange">Esempio</font>
> Si supponga che si voglia calcolare la probabilità che la somma sia $3$, ovvero $p_{Z}(3)$. 
> 
> Questo può accadere in molti modi.
> ![[Pasted image 20250319133604.png]]
> 
> La probabilità ricercata sarà uguale alla *somma delle probabilità di tutti i possibili modi in cui questo evento può accadere* (ovvero $x+y=3$).
> 
> $$\begin{align}p_{Z}(3)&=\dots + \mathbb{P}(X=0, Y=3) + \mathbb{P}(X=1, Y=2) + \dots \\ &= \text{[dato che X e Y sono indipendenti]} \\ &=\dots + p_{X}(0)\cdot p_{Y}(3)+p_{X}(1)\cdot p_{Y}(2)+\dots\end{align}$$

> Quindi, in generale, si ha la seguente **formula di convoluzione**:
> $$\begin{align}\color{#CC241D}p_{Z}(z)&=\sum_{x} \mathbb{P}(X=x,Y=z-x)\\&= \text{[dato che X e Y sono indipendenti]}\\&=\color{#CC241D} \sum_{x} [p_{X}(x)\cdot p_{Y}(z-x)]\end{align}$$

Essenzialmente, la convoluzione prende in input la densità discreta $p_{X}$ e $p_Y$ di due variabili aleatorie $X$ e $Y$ e *calcola una nuova densità discreta* $p_Z$ appartenente alla variabile aleatoria $Z$.

> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20250319135220.png]]
> 
> Per trovare $p_Z(3)$ si possono avere, per esempio, due metodi:
> - $p_{X}(1)\cdot p_{Y}(2)=\frac{1}{3}\cdot \frac{3}{6}=0.16$
> - $p_{X}(4)\cdot p_{Y}(-1)=\frac{2}{3}\cdot \frac{2}{6}=0.22$
> 
> Quindi, in questo caso specifico si ha che $$p_Z(3)=0.16+0.22=0.38$$

## Somme di VA Continue

>Consideriamo due variabili aleatorie $X$ e $Y$. Queste devono essere: 
>- [[041 Variabili Aleatorie Indipendenti|indipendenti]]
>- *[[034 Variabili Aleatorie Continue|continue]]*
>- [[040 Funzioni di Distribuzione Congiunte#Variabili Aleatorie Assolutamente Continue|assolutamente continue]]
>- devono avere una [[034 Variabili Aleatorie Continue#Funzione di Densità|densità di probabilità]] nota
>
>Supponiamo che si voglia calcolare 
$$\color{#CC241D}Z=X+Y$$

La situazione è **analoga per il caso discreto**.

Si imposta una funzione di somma $$\color{#61AFEF}g(X,Y)=X+Y$$
La probabilità dell'evento $f_{Z}(z)$ sarà uguale alla *somma delle probabilità di tutti i possibili modi in cui questo evento può accadere*, ovvero ad un [[066 Integrali|integrale]].

> [!example]+ <font color="orange">Esempio</font>
> Si supponga che si voglia calcolare la probabilità che la somma sia $3$, ovvero $f_{Z}(3)$. 
> 
> Questo può accadere in molti modi, segnati **da una retta**.
> ![[Pasted image 20250319140711.png]]
> 


> Quindi, in generale, si ha la seguente **formula di convoluzione continua**:
> $$\color{#CC241D}f_{Z}(z)=\int_{-\infty}^{+\infty} f_{X}(x)\cdot f_{Y}(z-x)\,dx$$

Inoltre:
$$f_{Z|X}(z|x)=f_{Y}(z-x)$$


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
- [MIT OpenCourseWare - Somma di Variabili Aleatorie Indipendenti Discrete](https://www.youtube.com/watch?v=zbu8KQx9bqM)